# Quiz 2 Answers
**Name:** Shaan Desai
**Date:** 11 March 2026



## Q1
D) The component should emit an event to the parent requesting the change, and the parent should update its own data in response.


## Q2
New code below:
```js
state: () => ({
    questions: [],
    currentIndex: 0,
    score: 0,
    gameState: 'start',    // 'start' | 'playing' | 'end'
    selectedAnswer: null,  // index of the button the player clicked, or null
    streak: 0,
    bestStreak: 0,
    lastAnswerCorrect: null,
})

submitAnswer(index) {
  if (index === this.questions[this.currentIndex].correct) {
    this.score++;
    this.lastAnswerCorrect = true;
  } else {
    this.lastAnswerCorrect = false;
  }
  this.nextQuestion();
}
```
`QuestionCard` should read `lastAnswerCorrect` from the store instead of receiving it as a prop from `App.vue` to avoid messy, complicated prop drilling. If `lastAnswerCorrect` is passed down as a prop, then it has to be kept track of at every level, threading through the layers even when intermediate parts don't need the value. In addition, this means that `QuestionCard` is kept more independent as it can retrieve its needed data without needing a prop from a parent. Keeping it in the centralized store so that it's easily accessible from `QuestionCard` keeps things clean without having to do prop drilling.


## Q3
C) All component instances that call useGameStore() share the same state object in memory, meaning a state change in one component affects all others simultaneously.


## Q4
**Part A:** The router is not rendering anything because the special `<router-view>` tag is not included in the template. There's nothing included that can render the `GameView` component correctly, as there's nothing telling the template that `/play` should display `GameView`. Here's what the corrected template would be:
```html
<template>
  <div>
    <h1>QuizBlitz</h1>
    <router-view/>
  </div>
</template>
```
Including `<router-view>` automatically handles routing when navigating to new pages, and will automatically display `GameView` when navigating to `/play`. `<router-view>` will display the correctly matched component based on the defined routes.


## Q5
B) Approach B, because ScoreBoard only displays data it receives — keeping it decoupled from the store makes it easier to reuse or test in isolation.


## Q6
If `currentIndex` is set to `state.questions.length`, then the `currentQuestion` getter will not return a valid question, as we are indexing out of bounds of the questions array. Rather, it will return undefined. The error occurs because we're trying to access `currentQuestion.text`, but there is no question at that index, so we won't be able to get any text out and an error will be thrown. Here's a better implementation of `nextQuestion` that would check if we're already on the last question and end the game accordingly rather than increasing the index.
```js
nextQuestion() {
    this.selectedAnswer = null
    this.timeLeft = 15
    if (this.currentIndex >= this.questions.length - 1) {
        this.gameState = 'end'
    } else {
        this.currentIndex++
    }
}
``` 
`nextQuestion` is a better place to fix rather than the getter, since the core of the issue is in `nextQuestion`. We should never be able to set our `currentIndex` to something out of bounds, so the way to fix this is to manually check it in `nextQuestion` and never get to that point.


## Q7
B) Using index as :key is acceptable here because the answers array for a given question does not change while the component is mounted; the risk of key-related bugs only arises when the list can be reordered or items deleted.


## Q8
**Local file approach**

Advantage: There's no overhead to retrieving the list of questions since they're being locally accessed; we don't need to wait for a server to respond to get our questions.

Disadvantage: There's no way for questions to be changed on the fly unless we go into the codebase and change them manually.

**Remote API approach**

Advantage: Since they are stored in a separate server, we can easily change questions on the fly if we want to add or modify any. This approach makes it very extensible.

Disadvantage: There's overhead to using the Fetch API and relying on a server to deliver the content compared to just accessing a local file. This approach could be slightly slower.

**My choice and reasoning:** For this app at the current state of the project, I would choose the local file approach. Fetching from a server might be good to have in a big production environment, but when we're scaffolding like this, it's a lot easier to just import directly from another JS file rather than trying to incorporate an external server already. For example, in the store action `startGame`, right now we're just able to directly reference our `questions` object for our `questions` property. If we were to switch to using Fetch, we'd have to do the entire fetching logic just to get our `questions` property populated.


## Q9
B) v-show hides elements with display: none but keeps them mounted, so startGame() would need to defend against being called while a game is already in progress, and the game state could persist unexpectedly between screen transitions.


## Q10
**useGameStore.js changes:**
Adding a `timeLeft` and an internal `_timeInterval` state properties so we can keep track of how much time is left on the timer and the JS interval object:
```js
state: () => ({
    questions: [],
    currentIndex: 0,
    score: 0,
    gameState: 'start',    // 'start' | 'playing' | 'end'
    selectedAnswer: null,  // index of the button the player clicked, or null
    timeLeft: 15,
    _timerInterval: null,  // internal — managed by the store only
    streak: 0,
    bestStreak: 0,
})
```
New actions for starting the timer, stopping it, and regularly "ticking" it.
```js
_startTimer() {
    this._stopTimer();
    this._timerInterval = setInterval(() => {
        this.tick();
    }, 1000);
},
_stopTimer() {
    if (this._timerInterval) {
        clearInterval(this._timerInterval);
        this._timerInterval = null;
    }
},
tick() {
    if (this.timeLeft > 0) {
        this.timeLeft--;
    } else {
        this.nextQuestion();
    }
}
```
Finally, we must add `_startTimer` calls to `startGame` and `nextQuestion` and `_stopTimer` calls to `submitAnswer` to make sure that the timer is appropriately stopped and started when moving from question to question.

**QuestionCard.vue changes (template only — to display the countdown):**
We add a timer bar that changes its width and color depending on the stored `timeLeft` value. This will automatically updates as the interval runs, so the countdown will display properly.
```html
<div class="timer-bar">
    <div
    class="timer-fill"
    :style="{ width: ((this.store.timeLeft / 15) * 100) + '%' }"
    :class="{ urgent: store.timeLeft <= 5 }"
    ></div>
</div>
```

**Why the timer logic belongs in the store, not the component:**
Timer logic belongs in the store because the question timer is something that multiple components rely on and react to, and a crucial part of the game flow. Keeping it centralized in the store means it's shared state that can be accessed by any component at any level. In addition, since the store is centralized and is running the whole time, it can manage the timer without anything interfering. if the timer logic was tied to the component, then it's reliant on the component's own lifecycle, which could lead to logic issues with the timer functioning properly. The timer logic belongs in the centralized store so that the intervals can be correctly handled without interference or component lifecycle risks.
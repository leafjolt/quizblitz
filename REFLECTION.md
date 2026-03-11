# Reflection
## Q1 - Props
A prop is a value passed down from parent to child component, that acts as part of the initial state for that child. Data flows down from the parent to the child. Props are better than importing `useGameStore` for presentational components like this since a presentational component like QuestionCard is specifically focused on showing something in the UI. It doesn't need the entire game state and it's overkill to retrieve the entire state every time, so it's much more efficient and cleaner to pass in the data it needs for its intended purpose.

## Q2 - $emit
When `$emit` is called, it sends an event/signal from a child component to its parent(s). The parent listens for it using a special Vue listener with "@". For example, if a child is emitting an `answer` event, we would put `@answer` in the template for the parent component as the event listener. If you forgot to handle the emitted event, nothing would happen; there would be no handler that listened for that event so there would be no explicit way for the app to handle it. `submitAnswer` would not get called, and the game would not progress.

## Q3 - Pinia store
As the component tree grows, you would have to pass that state down more and more components through props, leading to messy code and a complicated data flow down the component tree. Many components along the way would have to own data that they don't even need. This would be prop drilling, as props are being passed down layers of components until they reach the component that requires them. Moving the state into Pinia using `useGameStore` solves this issue by making state something that is centralized and accessible at any point from any component down the tree.

## Q4 - Vue Router
A traditional multi-page website will reload the entire page every time you route to a new page. A single-page application will handle routing so that when you navigate to a new page, the browser doesn't have to reload the entire site. `router-view` is a special component that displays only the component for the current page the user is on. Navigating between routes doesn't reload the page since the new "loading" of pages is handled entirely by JavaScript replacing the components of the page, rather than reloading and pulling from the server again.

## Q5 - v-if vs. v-show
`v-if` actually adds and removes the HTML it's on based on the conditional statement passed in, while `v-show` just toggles `display: none` but still keeps the elements around even if the conditional statement is not matched. `v-if` is the better choice here because we don't need elements of other pages to stick around when we don't need them; it's cleaner and more efficient to have them actually removed from the DOM with `v-if`.
<template>
    <div>
        <h1>Welcome back to QuizBlitz!</h1>
        <div>
            <label>
                Email:
                <input type="email" v-model="email" class="textInput" />
            </label>
        </div>
        <div>
            <label>
                Password:
                <input type="password" v-model="password" class="textInput" />
            </label>
        </div>
            
        <button @click="handleLogin" class="loginBtn">Login</button>
    </div>
</template>

<script>
import { useGameStore } from '../stores/gameStore';

export default {
    name: 'LoginView',
    setup() {
        return { store: useGameStore() }
    },
    data() {
        return {
            email: '',
            password: '',
        };
    },
    methods: {
        async handleLogin() {
            await this.store.login(this.email, this.password);
            this.$router.push({ name: 'home' })
        }
    }
};
</script>

<style scoped>
.loginBtn {
  padding: 0.5rem 2rem;
  margin: 1rem;
  font-size: 1.2rem;
  font-weight: 700;
  letter-spacing: 1px;
  color: #fff;
  background: linear-gradient(135deg, #667eea, #764ba2);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  box-shadow: 0 8px 24px rgba(102, 126, 234, 0.45);
  transition: transform 0.15s ease, box-shadow 0.15s ease;
}

.textInput {
    font-family: inherit;
    border-radius: 20px;
    border: none;
    padding: 0.5rem;
    margin: 0.5rem;
}
</style>
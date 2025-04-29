<template>
  <div class="login">
    <h2>Вход </h2>
    <form @submit.prevent="handleLogin">
      <div>
        <label for="email">Email: </label>
        <input type="email" id="email" v-model="email" required />
      </div>
      <div>
        <label for="password">Пароль: </label>
        <input type="password" id="password" v-model="password" required />
      </div>
      <button type="submit">Войти</button>
      <router-link to="/registration">
        <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
          <span>Регистрация</span>
        </li>
      </router-link>
    </form>
    <p v-if="error" class="error">{{ error }}</p>
  </div>
</template>

<script>
import { ref } from "vue"
import axios from "axios"
import { useRouter } from "vue-router" 

export default {
  name: "Login",
  setup() {
    const email = ref("")
    const password = ref("")
    const error = ref("")
    const router = useRouter()

    const handleLogin = async () => {
      error.value = ""
      try {
        const response = await axios.post("https://fc92f27366340adc.mokky.dev/auth", {
          email: email.value,
          password: password.value,
        })
        const token = response.data.token
        localStorage.setItem("apiToken", token)
        router.push({ name: "Home" })
        
      } catch (err) {
        console.error(err)
        error.value = "Ошибка авторизации. Проверьте введенные данные."
      }
    }

    return {
      email,
      password,
      handleLogin,
      error,
    }
  },
}
</script>

<style scoped>
.login {
  max-width: 400px;
  margin: 2rem auto;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.error {
  color: red;
  margin-top: 10px;
}
.submit{

}
.new{

}
</style>

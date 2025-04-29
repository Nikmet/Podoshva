<template>
    <div class="login">
      <form @submit.prevent="handleLogin">
        <div>
          <label for="name">Имя: </label>
          <input type="nick" id="name" v-model="name" required />
        </div>
        <div>
          <label for="email">Email: </label>
          <input type="email" id="email" v-model="email" required />
        </div>
        <div>
          <label for="password">Пароль: </label>
          <input type="password" id="password" v-model="password" required />
        </div>
        <button type="submit">Зарегистрироваться</button>
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
      const name = ref("")
      const email = ref("")
      const password = ref("")
      const error = ref("")
      const router = useRouter()
  
      const handleLogin = async () => {
        error.value = ""
        try {
          const response = await axios.post("https://fc92f27366340adc.mokky.dev/register", {
            name: name.value,
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
        name,
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
  
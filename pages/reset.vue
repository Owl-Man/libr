<template>
    <div class="page-wrapper">
      <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
      <header>
        <h1>Libria</h1>
      </header>
  
      <div class="container">
        <div class="tabs">
          <nuxt-link to="/login">Вход</nuxt-link>
          <nuxt-link to="/registration">Регистрация</nuxt-link>
          <nuxt-link to="/reset" class="active">Сброс пароля</nuxt-link>
        </div>
  
        <form @submit.prevent="register">
          <div class="form-group">
            <label for="email">Email</label>
            <input type="email" id="email" v-model="form.email" placeholder="Введите email" required />
          </div>
  
          <button type="submit">Отправить код для сброса пароля</button>
        </form>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        form: {
          username: '',
          email: '',
          password: ''
        }
      };
    },
    methods: {
      register() {
        // Логика отправки данных регистрации на сервер
        console.log('Регистрация', this.form);
        alert(
            `Пользователь ${this.form.username} зарегистрирован!`
          );
      }
    }
  };
  </script>
  
  <style scoped>
  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  
  .page-wrapper {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    background-color: #f5f5f5;
  }
  
  .container {
    background-color: #fff;
    padding: 20px 30px;
    border-radius: 10px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    width: 100%;
    max-width: 400px;
    text-align: center;
  }
  
  h1 {
    font-size: 24px;
    margin-bottom: 20px;
    color: #333;
  }
  
  .tabs {
    display: flex;
    justify-content: center;
    border-bottom: 2px solid #ddd;
    margin-bottom: 20px;
  }
  
  .tabs a {
    text-decoration: none;
    color: #666;
    padding: 10px 20px;
    transition: color 0.3s;
  }
  
  .tabs a.active {
    color: #007bff;
    border-bottom: 2px solid #007bff;
  }
  
  .social-buttons {
    display: flex;
    justify-content: center;
    gap: 10px;
    margin: 20px 0;
  }
  
  .social-buttons img {
    width: 40px;
    height: 40px;
    cursor: pointer;
    transition: transform 0.3s;
  }
  
  .social-buttons img:hover {
    transform: scale(1.1);
  }
  
  .form-group {
    margin-bottom: 15px;
    text-align: left;
  }
  
  label {
    display: block;
    font-size: 14px;
    color: #555;
    margin-bottom: 5px;
  }
  
  input {
    width: 100%;
    padding: 10px;
    font-size: 14px;
    border: 1px solid #ddd;
    border-radius: 5px;
  }
  
  button {
    width: 100%;
    padding: 10px;
    background-color: #007bff;
    color: #fff;
    border: none;
    border-radius: 5px;
    font-size: 16px;
    cursor: pointer;
    transition: background-color 0.3s;
  }
  
  button:hover {
    background-color: #0056b3;
  }
  </style>
  
  <style lang="scss">
  .page-wrapper h1 {
    font-size: 40;
  }
  </style>
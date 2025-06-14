<template>
  <div class="page-main">
    <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
    <div class="page-container">
      <div class="tabs">
        <button @click="changeLayout('6')" class="tab-button">Вход</button>
        <button @click="changeLayout('5')" class="tab-button active">Регистрация</button>
        <button @click="changeLayout('7')" class="tab-button">Сброс пароля</button>
      </div>

      <form @submit.prevent="register">
        <div class="form-group">
          <label for="username">Имя пользователя</label>
          <input type="text" id="username" v-model="form.username" placeholder="Введите имя пользователя" required />
        </div>

        <div class="form-group">
          <label for="email">Email</label>
          <input type="email" id="email" v-model="form.email" placeholder="Введите email" required />
        </div>

        <div class="form-group">
          <label for="password">Пароль</label>
          <input type="password" id="password" v-model="form.password" placeholder="Введите пароль" required />
        </div>

        <button type="submit">Зарегистрироваться</button>
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
    },
    changeLayout(layout) {
      this.$emit('change-layout', layout);
    }
  }
};
</script>

<style scoped>
.page-main {
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
  background-color: #f5f5f5;
  padding: 2rem;
}

.page-container {
  background-color: #fff;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  width: 100%;
  max-width: 400px;
  text-align: center;
}

.tabs {
  display: flex;
  justify-content: center;
  border-bottom: 2px solid #ddd;
  margin-bottom: 20px;
}

.tab-button {
  text-decoration: none;
  color: #666;
  padding: 10px 20px;
  transition: color 0.3s;
  background: none;
  border: none;
  cursor: pointer;
  font-size: 16px;
}

.tab-button.active {
  color: #007bff;
  border-bottom: 2px solid #007bff;
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

button[type="submit"] {
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

button[type="submit"]:hover {
  background-color: #0056b3;
}
</style> 
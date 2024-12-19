<template>
    <div class="registration-page">
      <link href="https://cdn.jsdelivr.net/npm/tailwindcss@2.1.2/dist/tailwind.min.css" rel="stylesheet">
      <h1>Регистрация</h1>
      <form @submit.prevent="registerUser" class="registration-form">
        <!-- Поле Имя -->
        <div class="form-group">
          <label for="first-name">Имя</label>
          <input
            type="text"
            id="first-name"
            v-model="form.firstName"
            placeholder="Введите имя"
            required
          />
        </div>
  
        <!-- Поле Фамилия -->
        <div class="form-group">
          <label for="last-name">Фамилия</label>
          <input
            type="text"
            id="last-name"
            v-model="form.lastName"
            placeholder="Введите фамилию"
            required
          />
        </div>
  
        <!-- Поле Дата рождения -->
        <div class="form-group">
          <label for="birth-date">Дата рождения</label>
          <input
            type="date"
            id="birth-date"
            v-model="form.birthDate"
            required
          />
        </div>
  
        <!-- Кнопка регистрации -->
        <button type="submit" class="submit-button">Зарегистрироваться</button>
      </form>
    </div>
  </template>



<script>
export default {
  data() {
    return {
      form: {
        firstName: "",
        lastName: "",
        birthDate: "",
      },
    };
  },
  methods: {
    registerUser() {
      // Простая проверка данных
      if (
        this.form.firstName.trim() &&
        this.form.lastName.trim() &&
        this.form.birthDate
      ) {
        alert(
          `Пользователь ${this.form.firstName} ${this.form.lastName} зарегистрирован!`
        );
        // Очистка формы
        this.form.firstName = "";
        this.form.lastName = "";
        this.form.birthDate = "";
      } else {
        alert("Пожалуйста, заполните все поля.");
      }
    },
  },
};
</script>



<style lang="scss">
.registration-page {
  max-width: 400px;
  margin: 50px auto;
  padding: 20px;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
  color: #333;
}

.registration-form {
  display: flex;
  flex-direction: column;
  gap: 15px;
}

.form-group {
  display: flex;
  flex-direction: column;
}

label {
  margin-bottom: 5px;
  font-weight: bold;
  color: #555;
}

input {
  padding: 10px;
  font-size: 14px;
  border: 1px solid #ddd;
  border-radius: 5px;
}

input:focus {
  outline: none;
  border-color: #007bff;
}

.submit-button {
  padding: 10px 15px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

.submit-button:hover {
  background-color: #0056b3;
}
</style>
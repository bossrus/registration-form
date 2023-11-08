<script>
import Vue from 'vue';

export default Vue.extend({
  name: 'FormComponent',
  data() {
    return {
      inputNameModel: '',
      inputEmailModel: '',
      inputTitleModel: '',
      inputPasswordModel: '',
      inputPasswordConfirmModel: '',
      inputPublicModel: true,
      inputPoliticsModel: true,

      showPassword: false,
      showConfirmPassword: false,

      formSubmitted: false,

      errors: {
        begin: ''
      },

      titles: [
        {
          value: 1, name: "менеджер"
        },
        {
          value: 2, name: "программист"
        },
        {
          value: 3, name: "курьер"
        },
        {
          value: 4, name: "директор"
        },
        {
          value: 5, name: "дворник"
        }
      ]

    }
  },
  created() {
    //моковый аксиос для имитации передачи данных
    this.axios = {
      async post(url, data) {

        return (url !== data);
      }
    }
  },
  computed: {
    inputModels() {
      return [
        this.inputNameModel,
        this.inputEmailModel,
        this.inputTitleModel,
        this.inputPasswordModel,
        this.inputPasswordConfirmModel,
        this.inputPublicModel,
        this.inputPoliticsModel,
      ];
    },
    cantSubmit() {
      return Object.keys(this.errors).length > 0;
    },
    errorsList() {
      return Object.values(this.errors).flat();
    }
  },
  watch: {
    inputModels() {
      this.validation();
    },
  },
  methods: {
    validation() {
      this.errors = {};
      if (this.isBlank(this.inputNameModel)) {
        this.errors['name'] = ['Поле "Имя" не может быть пустым'];
      }
      if (this.isBlank(this.inputEmailModel)) {
        this.errors['email'] = ['Поле "Email" не может быть пустым'];
      }
      if (this.isBlank(this.inputPasswordModel)) {
        this.errors['password'] = ['Поле "Пароль" не может быть пустым'];
      }
      if (this.isBlank(this.inputPasswordConfirmModel)) {
        this.errors['confirm'] = ['Поле "Повторите пароль" не может быть пустым'];
      }

      if (!this.checkPasswordMatch()) {
        this.errors['passwords'] = ['Пароли не совпадают'];
      }

      if (!this.inputPoliticsModel) {
        this.errors['politics'] = ['Для того, чтобы зарегистрироваться, вы должны согласиться с политикой конфиденциальности и с обработкой персональных данных'];
      }

      if (this.inputTitleModel === '') {
        this.errors.title = ['Выберите должность'];
      }

      if (!this.isCorrectEmail(this.inputEmailModel)) {
        if (!this.errors['email']) {
          this.errors['email'] = ['Некорректный email'];
        }
      }
    },

    isBlank(str) {
      return str.trim().length === 0;
    },

    isCorrectEmail() {
      const str = this.inputEmailModel.trim();

      let re = /^[a-zA-Z0-9-_]+(\.[a-zA-Z0-9_-]+)*@[a-zA-Z0-9-_]+\.[a-zA-Z0-9-_]+$/;
      return re.test(String(str).toLowerCase());
    },

    togglePasswordDisplay(what) {
      switch (what) {
        case 'password':
          this.showPassword = !this.showPassword;
          break;
        case 'confirm':
          this.showConfirmPassword = !this.showConfirmPassword;
          break;
      }
    },

    sendData() {
      this.validation();
      if (Object.keys(this.errors).length === 0) {
        const data = {
          public: this.inputPublicModel,
          username: this.inputNameModel,
          role: this.inputTitleModel,
          email: this.inputEmailModel,
          password: this.inputPasswordModel,
          password_repeat: this.inputPasswordConfirmModel
        };

        this.axios.post('https://api-url.com', data).then(() => {
          console.log("Mock POST request sent to https://api-url.com with data: ", data);
          this.formSubmitted = true;
        });


      }
    },
    checkPasswordMatch() {
      if (this.inputPasswordModel === '' || this.inputPasswordConfirmModel === '') {
        return true;
      }
      return this.inputPasswordModel === this.inputPasswordConfirmModel;
    }
  }
});
</script>

<template src="./FormComponent.html"></template>
<style lang="scss" scoped src="@/styles/main.scss"></style>
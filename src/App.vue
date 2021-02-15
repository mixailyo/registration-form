<template>
  <form action="/" @submit.prevent="checkForm" novalidate="true" class="form">
    <div v-if="!succesSubmit">
      <div class="form__title">
        <h1>Регистрация</h1>
        <p>Уже есть аккаунт? <a href="#">Войти</a></p>
      </div>

      <div class="form__name form__item">
        <label for="name">Имя</label>
        <input
          placeholder="Введите Ваше имя"
          id="name"
          v-model="name"
          type="text"
          name="name"
        />
        <span
          class="form__error"
          v-for="(error, index) in errorsName"
          :key="index"
          >{{ error }}</span
        >
      </div>

      <div class="form__email form__item">
        <label for="email">Email</label>
        <input
          placeholder="Введите Ваш email"
          id="email"
          v-model="email"
          type="email"
          name="email"
        />
        <span
          class="form__error"
          v-for="(error, index) in errorsEmail"
          :key="index"
          >{{ error }}</span
        >
      </div>

      <div class="form__tel form__item">
        <label for="tel">Номер телефона</label>
        <input
          placeholder="Введите Ваш номер телефона"
          id="tel"
          v-model="tel"
          type="tel"
          name="tel"
        />
        <span
          class="form__error"
          v-for="(error, index) in errorsTel"
          :key="index"
          >{{ error }}</span
        >
      </div>

      <div class="form__select">
        <span class="form__select-label">Язык</span>
        <p
          @click="showOptions = !showOptions"
          :class="{ active: showOptions, selected: selectedOptions }"
          class="form__select-placeholder"
        >
          {{ selectPlaceholder }}
        </p>
        <span
          class="form__error"
          v-for="(error, index) in errorSelect"
          :key="index"
          >{{ error }}</span
        >
        <div v-if="showOptions" class="form__select-options">
          <p
            @click="selectOption(option)"
            class="form__select-option"
            v-for="option in options"
            :key="option.value"
          >
            {{ option.name }}
          </p>
        </div>
      </div>

      <div class="form__accept">
        <span @click="accepted = !accepted"
          ><i :class="{ active: accepted }"></i
        ></span>
        <p>Принимаю <a href="#">условия </a>использования</p>
      </div>

      <div class="form__submit">
        <input type="submit" value="Зарегистрироваться" />
      </div>
    </div>

    <div v-if="succesSubmit" class="form__succes">
      <p>✔️</p>
      <p>Ваша заявка отправлена!</p>
    </div>
  </form>
</template>

<script>
export default {
  data() {
    return {
      succesSubmit: false,
      errorsName: [],
      errorsEmail: [],
      errorsTel: [],
      errorSelect: [],
      name: null,
      email: null,
      tel: null,
      selectPlaceholder: "Язык",
      selectedOptions: false,
      showOptions: false,
      accepted: false,
      options: [
        { value: "1", name: "Русский" },
        { value: "2", name: "Английский" },
        { value: "3", name: "Китайский" },
        { value: "4", name: "Испанский" },
      ],
    };
  },
  methods: {
    checkForm: function (e) {
      this.errorsName = [];
      this.errorsEmail = [];
      this.errorsTel = [];
      this.errorSelect = [];

      if (!this.name) {
        this.errorsName.push("Укажите имя");
      } else if (!this.validName(this.name)) {
        this.errorsName.push("Укажите корректное имя");
      }

      if (!this.email) {
        this.errorsEmail.push("Укажите электронную почту");
      } else if (!this.validEmail(this.email)) {
        this.errorsEmail.push("Укажите корректный адрес электронной почты");
      }

      if (!this.tel) {
        this.errorsTel.push("Укажите номер телефона");
      } else if (!this.validTel(this.tel)) {
        this.errorsTel.push("Укажите корректный номер телефона");
      }

      if (!this.selectedOptions) {
        this.errorSelect.push("Выберите язык");
      }

      if (
        !this.errorsName.length &&
        !this.errorsEmail.length &&
        !this.errorsTel.length &&
        !this.errorSelect.length &&
        this.accepted
      ) {
        this.succesSubmit = true;
        return true;
      }

      e.preventDefault();
    },
    validName: function (name) {
      var re = /^[a-zA-Zа-яёА-ЯЁ\s\-]+$/u;
      return re.test(name);
    },
    validEmail: function (email) {
      var re = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return re.test(email);
    },
    validTel: function (tel) {
      var re = /^(\s*)?(\+)?([- _():=+]?\d[- _():=+]?){10,14}(\s*)?$/;
      return re.test(tel);
    },
    selectOption(option) {
      this.selectPlaceholder = option.name;
      this.showOptions = false;
      this.selectedOptions = true;
    },
  },
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@400;700&display=swap");

* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

a {
  text-decoration: none;
  color: #0880ae;
}

body {
  background-color: rgb(236, 236, 236);
  font-family: "IBM Plex Sans", sans-serif;
  font-style: normal;
  font-weight: 500;
  font-size: 16px;
  color: #2c2738;
}

form {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  width: 460px;
  padding: 40px 30px;
  background: #ffffff;
  box-shadow: 0px 12px 24px rgba(44, 39, 56, 0.02),
    0px 32px 64px rgba(44, 39, 56, 0.04);
  border-radius: 24px;

  label {
    display: block;
    line-height: 1.3;
    color: #756f86;
    margin: 34px 0 8px 0;
  }

  input {
    display: block;
    width: 100%;
    height: 52px;
    padding: 16px;
    border: 1px solid #dbe2ea;
    box-shadow: 0px 4px 8px rgba(44, 39, 56, 0.04);
    border-radius: 6px;
    outline: none;
  }

  *::placeholder {
    color: #7c9cbf;
  }

  .form__item {
    position: relative;
  }

  .form__error {
    position: absolute;
    bottom: -20px;
    left: 0;
    color: #ff7171;
    font-size: 14px;
    line-height: 1.29;
  }

  .form__title {
    margin-bottom: 54px;

    h1 {
      margin-bottom: 8px;
      font-weight: bold;
      font-size: 34px;
      line-height: 1.29;
    }

    p {
      line-height: 1.37;
    }
  }

  .form__select {
    margin: 34px 0;
    position: relative;

    &-label {
      line-height: 1.3;
      color: #756f86;
    }

    p {
      width: 100%;
      height: 52px;
      padding: 16px;
      cursor: pointer;
    }

    &-placeholder,
    &-options {
      background-color: #fff;
      border: 1px solid #dbe2ea;
      box-shadow: 0px 4px 8px rgba(44, 39, 56, 0.04);
      border-radius: 6px;
      margin-top: 8px;
    }

    &-placeholder {
      position: relative;
      color: #7c9cbf;

      &::after {
        content: "";
        position: absolute;
        top: 25%;
        right: -10%;
        width: 15px;
        height: 15px;
        border-top: 2px solid #0880ae;
        border-right: 2px solid #0880ae;
        transform: rotate(135deg);
        margin-right: 60px;
      }

      &.active {
        border: 2px solid #0880ae;
        color: #2c2738;
      }

      &.selected {
        color: #2c2738;
      }
    }

    &-options {
      position: absolute;
      top: 80px;
      left: 0;
      width: 100%;
      z-index: 999;
    }

    &-option {
      color: #756f86;
      &:hover {
        background-color: #ebf4f8;
      }
    }
  }

  .form__succes {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    font-weight: bold;
    text-align: center;
    p:first-child {
      font-size: 60px;
    }
  }

  .form__accept {
    margin-bottom: 40px;
    position: relative;
    display: flex;
    align-items: center;
    span {
      display: inline-block;
      width: 28px;
      height: 28px;
      border: 2px solid #0880ae;
      box-sizing: border-box;
      box-shadow: 0px 4px 8px rgba(44, 39, 56, 0.04);
      border-radius: 4px;
      cursor: pointer;
      position: relative;
      i {
        position: absolute;
        top: 6px;
        left: 4px;
        width: 15px;
        height: 7px;
        border-top: 2px solid #0880ae;
        border-right: 2px solid #0880ae;
        transform: rotate(135deg);
        display: none;
        &.active {
          display: block;
        }
      }
    }
    p {
      display: inline;
      margin-left: 8px;
    }
  }

  .form__submit {
    input {
      background: #0880ae;
      box-shadow: 0px 2px 4px rgba(44, 39, 56, 0.08),
        0px 4px 8px rgba(44, 39, 56, 0.08);
      border-radius: 6px;
      color: #ebf4f8;
    }
  }
}

@media screen and (max-width: 485px) {
  form {
    width: 92%;
    padding: 20px 15px;

    .form__title {
      text-align: center;
      margin: 0;
    }

    .form__item {
      label {
        margin-top: 20px;
      }
    }

    .form__select {
      margin: 20px 0;
    }

    .form__accept {
      margin-bottom: 20px;
    }
  }
}

@media screen and (max-height: 800px) {
  form {
    position: static;
    transform: translate(0);
    margin: 15px auto;
  }
} ;
</style>

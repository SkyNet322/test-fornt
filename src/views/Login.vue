<template>
  <div class="login-page">
    <div class="login-page__container">
      <div class="login-page__logo c-icon">
        <svg>
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M0 14.5349C0 10.6627 1.51145 7.01671 4.25839 4.26557C7.00282 1.51444 10.6416 0 14.5061 0H48V7.3862H14.5061C12.6134 7.3862 10.8247 8.13274 9.47131 9.48757C8.11665 10.8462 7.37159 12.6384 7.37159 14.5349L7.32769 26.9332C6.98526 30.226 4.508 32.8829 1.30448 33.373L0 33.5729V14.5349Z"
            fill="#E30613"
          ></path>
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M0 48.0002V40.614H33.4939C35.3879 40.614 37.1753 39.8662 38.5274 38.5101C39.8808 37.1553 40.6272 35.3643 40.6272 33.4666L40.6359 21.6979C40.6359 18.194 43.2411 15.1538 46.6968 14.6272L48 14.4286V33.4666C48 37.3388 46.4886 40.9847 43.7416 43.7346C40.9972 46.4857 37.3584 48.0002 33.4939 48.0002H0Z"
            fill="#E30613"
          ></path>
          <path
            fill-rule="evenodd"
            clip-rule="evenodd"
            d="M28.3645 33.5983V24.0504L23.9982 29.2762L19.632 24.0504V33.5983H13.8496V14.8066H19.3284L23.9982 20.8493L28.668 14.8066H34.1469V33.5983H28.3645Z"
            fill="#E30613"
          ></path>
        </svg>
      </div>
      <div>
        <div class="login-page__title">Авторизация</div>
      </div>
      <form action="action_page.php" @submit.prevent="login">
        <div class="container">
          <div class="login-page__field">
            <input
              v-model="email"
              type="email"
              placeholder="Ваш ID"
              name="email"
              required
            />
          </div>
          <div class="login-page__field">
            <input
              v-model="password"
              type="password"
              placeholder="Ваш пароль"
              name="password"
              required
            />
          </div>

          <v-btn
            type="submit"
            class="c-button_theme_default c-button_disabled c-button_theme_default c-button_is-block"
            color="#e2e8ed"
            depressed
            ><div class="c-button__content">Войти</div></v-btn
          >
        </div>
      </form>
      <div class="login-page__caption">
        <a href="#" class="login-page__link">Забыли пароль или ID?</a>
      </div>
    </div>
  </div>
</template>

<script>
import gql from "graphql-tag";
import { onLogin } from "../vue-apollo.js";

export default {
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    login() {
      this.$apollo
        .mutate({
          mutation: gql`
            mutation($email: String!, $password: String!) {
              login(email: $email, password: $password)
            }
          `,
          variables: {
            email: this.email,
            password: this.password,
          },
        })
        .then((data) => {
          //Result
          onLogin(
            this.$apollo.provider.defaultClient,
            data.data.login
          );
          this.$router.push("/home");
        })
        .catch((error) => {
          // Error
          console.error(error);
          // We restore the initial user input
        });
    },
  },
};
</script>

<style>
.login-page {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.login-page__container {
  width: 394px;
}

.login-page__title {
  font-family: Montserrat, sans-serif;
  font-size: 24px;
  font-weight: 600;
  line-height: 32px;
  margin-top: 24px;
  text-align: center;
}

.login-page__logo {
  display: block;
  width: 48px;
  height: 48px;
  margin: 0 auto;
}

input {
  position: relative;
  display: block;
  height: 48px;
  border: 1px solid #e2e8ed;
  border-radius: 4px;
  overflow: hidden;
  padding: 10px;
  width: 100%;
}

.login-page__field + .login-page__field {
  margin-top: 16px;
}

.c-button_theme_default.c-button_disabled {
  background-color: #e2e8ed;
}

.c-button_theme_default {
  height: 48px;
  border-radius: 4px;
  margin-top: 20px;
  background-color: #e30613;
}

.c-button_is-block {
  display: block;
  width: 100%;
}

.c-button_theme_default.c-button_disabled .c-button__content {
  color: #8a9aac;
}

.c-button_theme_default .c-button__content {
  font-family: Montserrat, sans-serif;
  font-weight: 600;
  color: #fff;
}

.c-button__content {
  font-size: 14px;
  line-height: 1;
}

.v-btn {
  text-transform: capitalize;
}

.v-btn:not(.v-btn--round).v-size--default {
  padding: 0 24px;
  height: 48px;
}

.login-page__link {
  font-size: 13px;
  line-height: 20px;
  color: #8a9aac;
  text-decoration: none;
}

.login-page__caption {
  margin-top: 10px;
  text-align: center;
}
</style>
<script setup lang="ts">
import { reactive, onMounted } from 'vue';
import { RouterLink, RouterView } from 'vue-router'
import axios from 'axios';
// import HelloWorld from './components/HelloWorld.vue'

// defineProps<{
//   username: string,
//   password: string,
// }>()
const loginInfo = reactive({
  username : '',
  password : '',
})

// const formData = new FormData();
// formData.append('username', 'a');
// formData.append('password', 'a');

const login = ():void => {
  const formData = new FormData();
  formData.append('username', loginInfo.username);
  formData.append('password', loginInfo.password);
  axios.post('/login', formData)
    .then(res => {
      console.log("tomato");
      console.log(res);
    })
    .catch((error) => {console.log(error)});
}
onMounted(():void => {
  axios.defaults.baseURL = "http://localhost:8080"
   // CSRFトークンの取得
   axios.post('/api/csrf').then(response => {
    const csrfToken = response.data.token;
    console.log(csrfToken);
    // ここで取得したcsrfTokenを使ってリクエストを設定できます
    axios.defaults.headers.common['X-XSRF-TOKEN'] = csrfToken;
  });
})
</script>

<template>
  <table>
    <tr>
      <th class="title">
        <label for="username">ユーザーID</label>
      </th>
      <th>
        <input
          type="text"
          class="loginInput required_login"
          id="username"
          v-model="loginInfo.username"
          required
          maxlength="8"
        />
      </th>
    </tr>
    <tr>
      <th class="title">
        <label for="password">パスワード</label>
      </th>
      <th>
        <input
          type="password"
          class="loginInput required_login"
          id="password"
          v-model="loginInfo.password"
          required
          maxlength="12"
        />
      </th>
    </tr>
    <tr>
      <td colspan="2">
        <input
          type="button"
          @click="login()"
          id="btnLogin"
          class="login_button"
          name="btnLogin"
          value="ログイン"
        />
      </td>
    </tr>
  </table>

  <!-- <header>
    <img alt="Vue logo" class="logo" src="@/assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />

      <nav>
        <RouterLink to="/">Home</RouterLink>
        <RouterLink to="/about">About</RouterLink>
      </nav>
    </div>
  </header> -->

  <RouterView />
</template>

<style scoped></style>

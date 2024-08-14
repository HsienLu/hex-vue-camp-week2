<script setup>
import {ref} from "vue";
import axios from "axios";
const APIurl = "https://todolist-api.hexschool.io";
const userDatas = ref({
  email: "",
  password: "",
  nickname: "",
});
const userDatasSingIn = ref({
  email: "",
  password: "",
});
const checkLoginData = ref({
        Authorization: "",
});
const signupResMes=ref("");
const signinResMes=ref("");
const checkLoginMes=ref("");
const todoSignUp = () => {
 
fetch(`${APIurl}/users/sign_up`, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(userDatas.value),
})
.then(response => response.json())
.then(
  (data) =>{
    signupResMes.value=data;
    console.log(data)
  } 
  
    
  )
.catch(error => console.error('Error:', error));
};
const todoSignIn = () => {
 
 fetch(`${APIurl}/users/sign_in`, {
   method: 'POST',
   headers: {
     'Content-Type': 'application/json',
   },
   body: JSON.stringify(userDatasSingIn.value),
 })
 .then(response => response.json())
 .then(
   (data) =>{
     signinResMes.value=data;
     checkLoginData.value.Authorization=data.token;
     document.cookie = `token=${checkLoginData.value.Authorization}`;
     console.log(checkLoginData.value.Authorization)
   } 
   
     
   )
 .catch(error => console.error('Error:', error));
 };
 const checkLogin = () => {
 
  fetch(`${APIurl}/users/checkout`, {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `${document.cookie.replace(
  /(?:(?:^|.*;\s*)token\s*\=\s*([^;]*).*$)|^.*$/,
  "$1",
)}`,
    },
  })
  .then(response => response.json())
  .then(
    (data) =>{
      checkLoginMes.value=data;
      console.log(data)
    }   
    )
  .catch(error => console.error('Error:', error));
  console.log(checkLoginMes.value)
  };
// AAA7@AAA.com
// 111111
</script>

<template>
  {{ userDatas }}
  <main class="sing-up">
    <h1>註冊</h1>
    <div class="input-group">
      <label for="email">E-mail</label>
      <input
        type="email"
        name="email"
        placeholder="username"
        v-model="userDatas.email"
      />
    </div>
    <div class="input-group">
      <label for="password">Password</label>
      <input
        type="text"
        name="password"
        placeholder="password"
        v-model="userDatas.password"
      />
    </div>
    <div class="input-group">
      <label for="nickname">nickname</label>
      <input
        type="text"
        name="nickname"
        placeholder="nickname"
        v-model="userDatas.nickname"
      />
    </div>
    <button type="button" @click="todoSignUp">Sign Up</button>
    {{signupResMes}}
  </main>
  <main class="sing-in">
    <h1>登入</h1>
    <div class="input-group">
      <label for="email">E-mail</label>
      <input
        type="email"
        name="email"
        placeholder="username"
        v-model="userDatasSingIn.email"
      />
    </div>
    <div class="input-group">
      <label for="password">Password</label>
      <input
        type="text"
        name="password"
        placeholder="password"
        v-model="userDatasSingIn.password"
      />
    </div>

    <button type="button" @click="todoSignIn">Sign In</button>
    {{signinResMes}}
  </main>
  <main class="check-login">
   
    <h1>檢查登入</h1>
    {{checkLoginMes}}
    <button type="button" @click="checkLogin">檢查登入</button>
    
  </main>
  <main class="todo-list">
   
   <h1>取得代辦事項清單</h1>
   {{checkLoginMes}}
   <button type="button" @click="getTodoList">取得代辦事項清單</button>
   
 </main>
</template>

<style scoped>
.input-group {
  margin-bottom: 10px;
}
</style>

<template>
  <div v-if="userGoogle===false">
    <h3 class="p-top m-left">Loading...</h3>
  </div>
  
  <div v-if="userGoogle!==false" class="navbar">
    <h3 class="m-left">VUECHAT</h3>
    <button
      class="btn m-right"
      @click="googleAccess"
      v-if="!userGoogle"
    ><span class="material-icons">account_circle</span> Acceder
    </button>
    <button
      class="btn m-right"
      @click="logOut"
      v-if="userGoogle"
    >Salir <span class="material-icons">logout</span>
    </button>
  </div>
  <div v-if="userGoogle!==false" class="main">
    <Chat v-if="userGoogle" />
    <div v-else>
      <h3 class="p-top text-center">Inicia sesión para poder acceder al chat</h3>
    </div>
  </div>
  <div v-if="userGoogle!==false" class="footer">
    <FormAdd />
  </div>
</template>

<script setup>
import { GoogleAuthProvider, onAuthStateChanged, signInWithPopup, signOut } from 'firebase/auth'
import { auth } from './firebase';
import { ref } from 'vue';

import FormAdd from './components/FormAdd.vue';
import Chat from './components/Chat.vue';


const userGoogle = ref(false)

const googleAccess = async () =>{
  try {
      const provider = new GoogleAuthProvider();
      const {user} = await signInWithPopup(auth, provider)
      //console.log(user);
  } catch (error) {
    console.log(error)
  }
}

const logOut = async() => {
    await signOut(auth)
}

onAuthStateChanged(auth, user=>{
    console.log(user)
    userGoogle.value = user
})

</script>

<style>
*{
  box-sizing: border-box;
  margin:0;
  font-family: 'Space Grotesk', sans-serif;
}

html{
  background-color: #353941;
}

.navbar{
  position: fixed;
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: 8vh;
  width: 100%;
  background-color: #26282B;
}

.main{
  height: 90vh;
  background-color: #353941;
  margin: 0 auto;
  width: 90%;
}

.footer{
  /*background-color: #5F85DB;*/
  height: 10vh;
  margin: 0 auto;
  width:90%;
}

h3{
  color: #90B8F8 
}

.btn{
  padding: 10px 20px;
  background-color: #90B8F8;
  color: #26282B;
  border-color: #353941;
  border-radius: 4px;
  text-transform: uppercase;
  font-weight: bold;
  display: flex;
  align-items: center;
  transition: all .2s;
}

.btn:hover{
  cursor: pointer;
  box-shadow: 2px 2px 2px 1px #47d3f7;
}

.btn:active{
  background-image: linear-gradient(from #90B8F8 to #47d3f7);
}

.m-top{
  margin-top: 20px;
}

.p-top{
  padding-top: 50px;
}

.m-left{
  margin-left: 45px;
}

.m-right{
  margin-right: 45px;
}

.text-center{
  text-align: center;
  padding-top: 60px;
}
</style>

<!--COLORES-->
<!--
  #26282B
  #353941
  #5F85DB
  #90B8F8
-->

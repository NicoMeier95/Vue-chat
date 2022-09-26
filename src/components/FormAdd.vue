<template>
    <form class="full-height" @submit.prevent="sendMessage"> 
        <div class="form-control">
            <input 
            class="mx-auto full-width msg-input" 
            type="text" 
            placeholder="Ingresa tu mensaje"
            v-model="message"    
            >
            <span @click.prevent="sendMessage" class="material-icons color">send</span>
        </div>
    </form>
</template>

<script setup>
import { addDoc, collection } from "@firebase/firestore";
import { ref } from "@vue/reactivity"
import { auth, db } from "../firebase";

const message = ref('')

const sendMessage = async()=>{
    try {
        await addDoc(collection(db, 'chats'),{
            text: message.value,
            time: Date.now(),
            uid: auth.currentUser.uid,
            displayName: auth.currentUser.displayName
        })


        message.value = '';
    } catch (error) {
        console.log(error)
    }
     
}

</script>

<style scoped>
.mx-auto{
    margin: 5px auto;
}
.full-width{
    width:95%;
    padding: 15px 0;
}

.full-height{
    height: 5vh;
    align-items: center
}

.msg-input{
    border: 2px solid #26282b;
    border-radius: 4px;
    outline:none
}

.msg-input:active, .msg-input:focus {
    border: 2px solid #90b8f8;
    outline: none;
}

.form-control{
    display:flex;
    align-items: center;
}

.color{
    color: #90b8f8;
}
</style>
<template>
    <div class="p-top">
        <div 
            class="msg-box" 
            v-for="item in messages" 
            :key="item.id"
            :class="item.uid===userChat.uid && 'ml-auto enviado'"    
        >
            <div class="msg-title">{{item.displayName}}</div>
            <div class="msg-text">{{item.text}}</div>
        </div>
        

    </div>
</template>

<script setup>
import { collection, onSnapshot, orderBy, query} from "@firebase/firestore";
import { ref } from "@vue/reactivity";
import { auth, db } from "../firebase";

const userChat = ref(auth.currentUser);

const messages = ref([]);

const q = query(collection(db,'chats'), orderBy('time'));
const unsuscribe = onSnapshot(q, (snapshot)=>{
    snapshot.docChanges().forEach((change)=>{
        if(change.type==='added'){
            //console.log('Nuevo mensaje: ',change.doc.id , change.doc.data())
            messages.value.push({
                id: change.doc.id,
                ...change.doc.data()
            })
        }
    })
})


</script>

<style scoped>
.p-top{
    padding-top: 55px
}
.msg-box{
    width: 300px;
    background-color: #5f85db;
    margin-bottom: 15px;
    color: #f4f4f4;
    padding: 10px 20px;
    border-radius: 4px;
}

.msg-title {
    font-weight: bold;
    margin-bottom: 5px;
}

.ml-auto{
    margin-left: auto;
}

.mr-auto{
    margin-right: auto;
}

.enviado{
    background-color: #3b5a8b;
}
</style>
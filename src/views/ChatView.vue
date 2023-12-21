<script setup lang="ts">
import { ref, onMounted, watch } from 'vue'
import MyButton from '../components/MyButton.vue'
import MyInput from '../components/MyInput.vue'
import UserBoxVue from '../components/UserBox.vue';


let mychat = ref("")

const KEY = "chats"
const KEY_NAME = "name"

let currentName = ref<string>("")
let currentClass = ref("start")

interface Chat {
    name: string
    content: string
}

const chatStr = ref(localStorage.getItem(KEY) || '')
let chats = ref<Chat[]>([]);

function handleStorage(event: any) {
    if (event.key === KEY) {
        chatStr.value = event.newValue
    }
}
function increments() {
    let chat: Chat = { name: sessionStorage.getItem(KEY_NAME) || "", content: mychat.value };

    chats.value.push(chat);

    localStorage.chats = JSON.stringify(chats.value);
    mychat.value = "";
}

onMounted(() => {
    window.addEventListener('storage', handleStorage)
    const username = prompt("Enter username");

    if (username) {
        currentName.value = username;
        sessionStorage.setItem(KEY_NAME, username);
        currentClass.value = "end";

    }
    if (localStorage.chats) {
        chatStr.value = localStorage.chats
        chats.value = JSON.parse(chatStr.value);
    }
})

watch(chatStr, (newChatsStr) => {
    // yes, console.log() is a side effect
    console.log(newChatsStr);
    chats.value = JSON.parse(newChatsStr);
})
</script>


<style>
.chat-box {
    border: 1px solid #ccc;
    width: 100%;
    display: flex;
    flex-flow: column;
    width: 400px;
}

.form-box {
    border: 1px solid #ccc;
    min-height: 500px;
}

.bottom {
    padding: 4px;
}
</style>

<template>
    <h3>Welcome!</h3>
    <div class="chat-box">
        <div class="form-box">
            <div class="user-box" v-for="chat in chats" :key="chat.name">
                <UserBoxVue :name="chat.name == currentName ? 'Me' : chat.name" :message="chat.content"
                    :current-class="chat.name == currentName ? 'end' : 'start'"></UserBoxVue>
            </div>
        </div>
        <div class="bottom">
            <MyInput @response="(msg) => mychat = msg"></MyInput>
            <MyButton title="Send" @click="increments"></MyButton>
        </div>
    </div>
</template>
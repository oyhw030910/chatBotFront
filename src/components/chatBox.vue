<!-- ChatBox.vue -->

<template>
    <div class="box">
        <div class="chat-box">
            <div class="chat-history" ref="chatHistory">
                <div v-for="message in messages" :key="message.id" class="message" :class="{ 'user-message': message.isUser, 'bot-message': !message.isUser }">
                    <span>{{ message.text }}</span>
                </div>
            </div>
            <div class="chat-input">
                <input v-model="currentMessage" @keyup.enter="sendMessage" placeholder="Type your message..." />
                <button @click="sendMessage">Send</button>
            </div>
        </div>
        <div class="table">
            <span></span>
        </div>
    
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  export default {
    data() {
      return {
        messages: [],
        currentMessage: '',
      };
    },
    methods: {
      sendMessage() {
        if (this.currentMessage.trim() === '') return;
  
        this.messages.push({
          id: new Date().getTime(),
          text: this.currentMessage,
          isUser: true,
        });
        axios.post('http://127.0.0.1:8080/hello',{message:this.currentMessage}).then((result)=>{
          this.messages.push({
          id: new Date().getTime() + 1,
          text: result.data.message,
          isUser: false,
        });
        })
  
        
  
        this.currentMessage = '';
  
        // 滚动到最新消息
        this.$nextTick(() => {
          const chatHistory = this.$refs.chatHistory;
          chatHistory.scrollTop = chatHistory.scrollHeight;
        });
      },
    },
  };
  </script>
  
  <style scoped>
 .box{
    display: flex;
    flex: 1;
    
 }
 .table{
    background-image:url('./icons/1.png') ;
    background-size: cover;
 }
  .chat-box {
    width: 100%;
    height: 100vh;
    display: flex;
    flex-direction: column;
    background-image:url('./icons/1.png') ;
    background-size: cover;
  }
  
  .chat-history {
    flex: 1;
    overflow-y: auto;
    padding: 10px;
  }
  
  .message {
    display: flex;
    width: fit-content;
    margin-bottom: 10px;
    word-wrap: break-word;
    word-break: break-all;
    padding: 10px;
    border-radius: 8px;
    
  }
  
  .user-message {
    margin-left: auto;
    background-color: #3498db;
    color: #fff;
  }
  
  .bot-message {
    align-self: flex-start;
    background-color: #2ecc71;
    color: #fff;
  }
  
  .chat-input {
    display: flex;
    flex-shrink: 0;
    padding: 10px;
  }
  
  input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
  }
  
  button {
    padding: 10px;
    border: none;
    border-radius: 4px;
    background-color: #3498db;
    color: #fff;
    cursor: pointer;
  }
  </style>
  
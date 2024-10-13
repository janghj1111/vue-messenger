<template>
  <div id="app">
    유저이름: 
    <input
      v-model="userName"
      type="text"
    >
    내용: <input
      v-model="message"
      type="text"
      @keyup="sendMessage"
    >
    <div
      v-for="(item, idx) in recvList"
      :key="idx"
    >
      <h3>유저이름: {{ item.userName }}</h3>
      <h3>내용: {{ item.content }}</h3>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import Stomp from 'webstomp-client'
import SockJS from 'sockjs-client'

const userName = ref('')
const message = ref('')
const recvList = ref([])

let stompClient = null
let connected = ref(false)

const sendMessage = (e) => {
  if (e.keyCode === 13 && userName.value !== '' && message.value !== '') {
    send()
    message.value = ''
  }
}

const send = () => {
  console.log("Send message:" + message.value)
  if (stompClient && stompClient.connected) {
    const msg = { 
      userName: userName.value,
      content: message.value 
    }
    stompClient.send("/receive", JSON.stringify(msg), {})
  }
}

const connect = () => {
  const serverURL = "http://localhost:9092"
  let socket = new SockJS(serverURL)
  stompClient = Stomp.over(socket)
  console.log(`소켓 연결을 시도합니다. 서버 주소: ${serverURL}`)
  
  stompClient.connect(
    {},
    (frame) => {
      connected.value = true
      console.log('소켓 연결 성공', frame)

      // 구독 설정
      stompClient.subscribe("/send", (res) => {
        console.log('구독으로 받은 메시지 입니다.', res.body)
        recvList.value.push(JSON.parse(res.body))
      })
    },
    (error) => {
      console.log('소켓 연결 실패', error)
      connected.value = false
    }
  )
}

onMounted(() => {
  connect()
})
</script>

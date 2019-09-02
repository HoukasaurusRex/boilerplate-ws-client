<template>
  <div class="hello">
    <h1>Welcome to Simply Websockets Client</h1>
    <div v-for="(message, index) in messages" :key="index">
      <Message
        :from="message.from"
        :text="message.message"
        :createdAt="message.createdAt"
      />
    </div>
  </div>
</template>

<script>
import Message from './Message'
import { setInterval, clearTimeout } from 'timers'
export default {
  components: {
    Message
  },
  data() {
    return {
      messages: [],
      connected: false,
      reconnect: undefined
    }
  },
  methods: {
    connectWS() {
      const ws = new WebSocket('ws://localhost:9000')
      // const echo = JSON.stringify({ event: 'echo', text: 'hello WSS!' })
      const join = JSON.stringify({
        event: 'join',
        room: 'cool room',
        name: 'test'
      })
      ws.onopen = () => {
        ws.send(join)
        this.connected = true
        clearTimeout(this.reconnect)
      }
      ws.onmessage = res => {
        try {
          const body = JSON.parse(res.data)
          this.messages.push(body)
        } catch (err) {
          console.log({ res })
        }
      }
      ws.onclose = () => {
        console.warn('Connection closed')
        this.connected = false
        this.reconnect = setInterval(this.connectWS, 5000)
      }
    }
  },
  mounted() {
    this.connectWS()
  }
}
</script>

<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

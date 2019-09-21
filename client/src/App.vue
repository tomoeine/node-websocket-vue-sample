<template>
  <div id="app">
    <input
      id="message"
      type="text"
      v-model="message"
    >
    <input
      id="send-button"
      type="button"
      value="Send"
      @click="sendMessage"
    >

    <h5>Received Message</h5>
    <p id="received-message">
      {{ receivedMessage }}
    </p>
  </div>
</template>

<script>
  export default {
    data() {
      return {
        sock: null,
        message: '',
        receivedMessage: '',
      }
    },
    created() {
      this.sock = new WebSocket('ws://' + location.hostname + ':8080')
      this.sock.addEventListener('message', this.receiveMessage, false)
    },
    destroyed: function () {
      this.sock.removeEventListener('message', this.receiveMessage, false)
    },
    methods: {
      receiveMessage(e) {
        document.getElementById('received-message').innerHTML = e.data
      },
      sendMessage() {
        this.sock.send(this.message)
        this.message = ''
      },
    },
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>

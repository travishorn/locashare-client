<template>
  <div class="container my-3 my-md-5">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <router-link class="btn btn-secondary mb-3" to="/">Disconnect</router-link>

        <p>Share code: <span class="text-mono">{{ $route.params.shareCode }}</span></p>

        <p>Your position: {{ position }}</p>

        <p>TODO: If two or more clients are in the same "room" (have the same share code), they will share their position with each other. Check the console for position updates from other clients.</p>
      </div>
    </div>
  </div>
</template>

<script>
import io from 'socket.io-client';

export default {
  name: 'room',
  data() {
    return {
      socket: null,
      position: null,
      msg: '',
    };
  },
  methods: {
    updatePosition(position) {
      this.position = [position.coords.latitude, position.coords.longitude];
      if (this.socket !== null) this.socket.emit('updatePosition', this.position);
    },
  },
  mounted() {
    this.socket = io('http://localhost:8081');

    navigator.geolocation.getCurrentPosition(this.updatePosition);
    navigator.geolocation.watchPosition(this.updatePosition);

    this.socket.on('connect', () => {
      this.socket.emit('joinRoom', this.$route.params.shareCode);
    });

    this.socket.on('peerPositionUpdated', (data) => {
      console.log(`Peer position updated. socketId: ${data.id}. Position: ${data.position}`);
    });
  },
  destroyed() {
    this.socket.disconnect();
    this.socket = null;
  },
};
</script>

<style>
  .text-mono {
    font-family: monospace;
    font-size: 1.5rem;
  }
</style>

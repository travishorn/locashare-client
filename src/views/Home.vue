<template>
  <div class="container my-3 my-md-5">
    <div class="row justify-content-center">
      <div class="col-md-6">
        <form @submit.prevent="joinRoom">
          <div class="form-group">
            <label for="shareCode">Share code:</label>
            <input id="shareCode" class="form-control" v-model="shareCode" autofocus>
          </div>

          <button class="btn btn-primary btn-block">Join room</button>
        </form>

        <hr class="my-md-5">

        <button class="btn btn-secondary btn-block" @click="createRoom">Create new room</button>
      </div>
    </div>
  </div>
</template>

<script>
import generate from 'nanoid/generate';

export default {
  name: 'home',
  data() {
    return {
      shareCode: '',
    };
  },
  methods: {
    joinRoom() {
      this.$router.push(`/room/${this.shareCode}`);
    },
    createRoom() {
      const shareCode = this.generateShareCode();
      this.$router.push(`/room/${shareCode}`);
    },
    generateShareCode() {
      return generate('23456789ABCDEFGHJKLMNPQRSTUVWXYZ', 5);
    },
  },
};
</script>

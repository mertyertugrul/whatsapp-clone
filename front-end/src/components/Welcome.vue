<template>
  <div class="welcome container">
    <h3>{{ msg }}</h3>
    <div class="card">
      <div class="card-content center-align">
        <h2>Welcome</h2>
        <form @submit.prevent="enterChat">
          <label for="name">Enter Your Name:</label>
          <input type="text" name="name" v-model="name" />
          <p v-if="feedback" class="red-text">{{ feedback }}</p>
          <button class="btn teal">Enter Chat</button>
        </form>
      </div>
    </div>
  </div>
</template>


<script>
export default {
  name: 'Welcome',
  props: {
    msg: String,
  },
  data() {
    return {
      name: null,
      feedback: null,
    };
  },
  methods: {
    enterChat() {
      if (this.name) {
        this.$router.push({ name: 'chat', params: { name: this.name } });
      } else {
        this.feedback = 'You need to enter your name to join';
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.welcome {
  max-width: 400px;
  margin-top: 100px;
}
.welcome h2 {
  font-size: 3em;
}
.welcome button {
  margin: 30px auto;
}
.welcome h3 {
  color: #009788;
  text-align: center;
}
</style>

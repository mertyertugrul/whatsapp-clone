<template>
  <div class="new-message">
    <form @submit.prevent="addMessage">
      <label for="new-message">New Message (Enter to add):</label>
      <input type="text" name="new-message" v-model="newMessage">
      <p class="red-text" v-if="feedback">{{ feedback }}</p>
    </form>
  </div>
</template>

<script>
import db from '../firebase/init';

export default {
  name: 'NewMessage',
  props: ['name'],
  data() {
    return {
      newMessage: null,
      feedback: null,
    };
  },
  methods: {
    addMessage() {
      if (this.newMessage) {
        // console.log(this.newMessage);
        db.collection('messages').add({
          content: this.newMessage,
          name: this.name,
          timeStamp: Date.now(),
        }).catch((err) => {
          console.log(err);
        });
        this.newMessage = null;
        this.feedback = null;
      } else {
        this.feedback = 'There is no message to sent.';
      }
    },
  },
};
</script>

<style>

</style>

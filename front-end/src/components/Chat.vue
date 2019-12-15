<template>
  <div class="chat container">
    <h2 class="center teal-text">Chat</h2>
    <div class="card">
      <div class="card-content">
        <ul class="messages" v-chat-scroll>
          <li v-for="message in messages" :key="message.id">
            <span class="teal-text">{{ message.name }} </span>
            <span class="grey-text text-darken-3">{{ message.content }} </span>
            <span class="grey-text time">{{ message.timeStamp }}</span>
          </li>
        </ul>
      </div>
      <div class="cart-action">
        <NewMessage :name="name"/>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
import NewMessage from './NewMessage.vue';
import db from '../firebase/init';

export default {
  name: 'Chat',
  props: ['name'],
  components: {
    NewMessage,
  },
  data() {
    return {
      messages: [],
    };
  },
  created() {
    const ref = db.collection('messages').orderBy('timeStamp');
    ref.onSnapshot((snapshot) => {
      snapshot.docChanges().forEach((change) => {
        if (change.type === 'added') {
          const { doc } = change;
          this.messages.push({
            id: doc.id,
            name: doc.data().name,
            content: doc.data().content,
            timeStamp: moment(doc.data().timeStamp).format('LLL'),
          });
        }
      });
    });
  },
};
</script>

<style scoped>
.chat h2{
  font-size: 2.6em;
  margin-bottom: 40px;
}
.chat span{
  font-size: 1.2em;
}
.chat .time{
  display: block;
  font-size: 0.8em;
}
.messages {
  max-height: 300px;
  overflow: auto;
}

.messages::-webkit-scrollbar{
  width: 3px;
}
.messages::-webkit-scrollbar-track{
  background: #ddd;
}
.messages::-webkit-slider-thumb{
  background: #aaa;
}
</style>

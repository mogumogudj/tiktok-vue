<script setup>
 // import ref
    import { ref, reactive, onMounted } from 'vue';

    let message = ref(""); // int, string or boolean
    let allMessages = reactive({
        data: ["Heel", "Veel", "Berichten"],
    }); // array or object

    let messages = ref([]);

    // function to send a message
    const sendMessage = () => {
        allMessages.data.push(message.value);
        message.value = "";
    };
    
    onMounted(async () => {
  try {
    const response = await fetch('https://lab5-p379.onrender.com/api/v1/messages/');
    const data = await response.json();
    messages.value = data;
  } catch (error) {
    console.error('Error fetching messages:', error);
  }
});

    </script>

<template>
  <div>
    <ul>
        <li v-for="message in allMessages.data">{{ message }}</li>
    </ul>

    <div>

        <div class="chat">
    <div v-for="message in messages" :key="message._id" class="message">
      <span class="username">{{ message.user }}:</span>
      <span class="text">{{ message.text }}</span>
    </div>
  </div>


        <!-- v-model is a shorthand for :value and @input event listener which allows us to update the value of the message variable -->
        <input v-model="message" type="text" placeholder="" />
        <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<style scoped>

.chat {
  overflow-y: auto;
  max-height: 300px;
  border: 1px solid #ccc;
  padding: 10px;
}

.message {
  margin-bottom: 10px;
}

.username {
  font-weight: bold;
  margin-right: 5px;
}

</style>

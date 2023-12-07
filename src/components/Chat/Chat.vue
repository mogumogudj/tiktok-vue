<script setup>
 // import ref
    import { ref, reactive, onMounted, onBeforeMount } from 'vue';

    let message = ref(""); // int, string or boolean
    let allMessages = reactive({
        data: ["Heel", "Veel", "Berichten"],
    }); // array or object

    let messages = ref([]);
    let newComment = ref('');
    const userCommentsKey = 'userComments';

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

// load user comments from local storage on component mount
onBeforeMount(() => {
  const storedComments = localStorage.getItem(userCommentsKey);
  if (storedComments) {
    messages.value = JSON.parse(storedComments).concat(messages.value);
  }
});



// function to add a new comment
const addMessage = () => {
  if (newComment.value.trim() !== '') {
    messages.value.unshift({ user: 'You', text: newComment.value });
    // Save user messages to local storage
    const updatedComments = JSON.stringify(messages.value.slice(0, 10)); // Keep only the latest 10 messages
    localStorage.setItem(userCommentsKey, updatedComments);
    newComment.value = ''; // Clear the input field after adding the message
  }
};



    </script>

<template>
  <div>
    <ul>
        <li v-for="message in allMessages.data">{{ message }}</li>
    </ul>

    <div>

        <div class="chat">

          <div class="add-comment">
      <input v-model="newComment" placeholder="Add your message" />
      <button @click="addMessage">Submit</button>
    </div>




    <div v-for="message in messages" :key="message._id" class="message">
      <span class="username">{{ message.user }}:</span>
      <span class="text">{{ message.text }}</span>
    </div>
  </div>


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

.add-comment {
  margin-bottom: 10px;
}

</style>

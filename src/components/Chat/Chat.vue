<script setup>
 // import ref
    import { ref, reactive, onMounted, onBeforeMount } from 'vue';

    let message = ref(""); // int, string or boolean

    let messages = ref([]);
    let newComment = ref('');
    const userCommentsKey = 'userComments';





    // function to send a message
    const sendMessage = async () => {
  try {
    const response = await fetch('https://lab5-p379.onrender.com/api/v1/messages/', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        user: 'You',
        text: newComment.value,
      }),
    });

    if (response.ok) {
      // Fetch the updated messages from the server after posting the new comment
      const updatedMessagesResponse = await fetch('https://lab5-p379.onrender.com/api/v1/messages/');
      const updatedMessages = await updatedMessagesResponse.json();

      // Update the local messages array with the new data
      messages.value = updatedMessages;

      // Save user messages to local storage
      const updatedComments = JSON.stringify(messages.value.slice(0, 10));
      localStorage.setItem(userCommentsKey, updatedComments);

      newComment.value = ''; // Clear the input field after adding the message

      console.log('Message sent successfully:', messages.value);
    } else {
      console.error('Failed to send message:', response.statusText);
    }
  } catch (error) {
    console.error('Error sending message:', error);
  }
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



    </script>

<template>
  <div>
    <!-- <ul>
        <li v-for="message in allMessages.data">{{ message }}</li>
    </ul> -->

    <div>

        <div class="chat">
    <div v-for="message in messages" :key="message._id" class="message">
      <span class="username">{{ message.user }}:</span>
      <span class="text">{{ message.text }}</span>
    </div>
        </div>


    <div class="add-comment">
      <input v-model="newComment" placeholder="Add your message" />
      <button @click="sendMessage">Submit</button>
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
  border-radius: 8px;
}

.message {
  margin-bottom: 10px;
  padding: 8px;
  border-radius: 8px;
  background-color: #f8f8f8;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.username {
  font-weight: bold;
  margin-right: 5px;
}

.add-comment {
  margin-top: 10px;
  margin-bottom: 10px;
  display: flex;
  justify-content: space-between;
}

input {
  flex: 1;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

button {
  margin-left: 8px;
  padding: 8px 16px;
  background-color: #00a4d8;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

</style>

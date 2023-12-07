<script setup>
 // import ref
    import { ref, reactive, onMounted } from 'vue';

    let message = ref(""); // int, string or boolean
    let allMessages = reactive({
        data: ["Heel", "Veel", "Berichten"],
    }); // array or object

    // function to send a message
    const sendMessage = () => {
        allMessages.data.push(message.value);
        message.value = "";
    };
</script>

<template>
  <div>
    <ul>
        <li v-for="message in allMessages.data">{{ message }}</li>
    </ul>

    <div>
        <!-- v-model is a shorthand for :value and @input event listener which allows us to update the value of the message variable -->
        <input v-model="message" type="text" placeholder="" />
        <button @click="sendMessage">Send</button>
    </div>
  </div>
</template>

<style scoped>

</style>

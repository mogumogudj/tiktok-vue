<script setup>

import { ref, reactive, onMounted } from 'vue';

// define emits
const emits = defineEmits(["update:videoDescription"]);


let videoUrl = ref("");
let videos = reactive({
    data: [],
});

let currentVideoIndex = ref(0); // Track the index of the current video

const switchToVideo = (index) => {
    if (index >= 0 && index < videos.data.length) {
        videoUrl.value = videos.data[index].video;
        emits("update:videoDescription", videos.data[index].description);
        currentVideoIndex.value = index;
    }
};

onMounted (() => {
    fetch("https://api.jsonbin.io/v3/b/6548ef9954105e766fcc2c15")
    .then((response) => response.json())
    .then((data) => {
        console.log(data);
        videos.data = data.record.videos;
        videoUrl.value = videos.data[0].video; // voor een tweede video moet die counter met 1 naar boven gaan en dan de video van die counter pakken
        
        // emit event 
        emits("update:videoDescription", videos.data[0].description);
    });
});


</script>

<template>
    <div class="video-player">
        <video :src="videoUrl" controls autoplay loop></video>

        <div class="video-buttons">
            <button  @click="switchToVideo(currentVideoIndex - 1)" class="prev-button">Previous Video</button>
            <button @click="switchToVideo(currentVideoIndex + 1)" class="next-button">Next Video</button>
        </div>
    </div>
</template>

<style scoped>
.video-player {
    position: relative;
}

video {
    width: 100%;
}

.video-buttons {
    position: absolute;
    top: 85%;
    transform: translateY(-50%);
    display: flex;
    justify-content: space-between;
    width: 100%;
}

.prev-button,
.next-button {
    background-color: #fff;
    border: none;
    color: #000;
    font-size: 24px;
    padding: 8px;
    cursor: pointer;
    outline: none;
}

.prev-button:hover,
.next-button:hover {
    background-color: #ddd;
}
</style>

<script setup lang="ts">

import axios from "axios";
import {ref} from "vue";
import {Message} from "@/message";


const url = "http://127.0.0.1:11434/api/generate"
const prompt = ref("")
const messages : Message[] = []

async function submitPrompt(){

  messages.push({
    "content": prompt.value,
    "author": "user"
  });

  await axios.post(url, {
    "model": "mistral",
    "prompt": prompt.value,
    "stream": false
  }).then((response) => {
    console.log(response.data.response)

    messages.push({
      "content": response.data.response,
      "author": "ia"
    })
  })

  prompt.value = ""; //clear input


}


</script>

<template>
  <h1>Pose une question </h1>
  <div class="">
    <form action="" method="" @submit.prevent="submitPrompt">
      <input type="text" v-model="prompt">
      <button type="submit">Send</button>
    </form>
  </div>
  <div v-for="message in messages">
    <div class="card">
      <p>{{message.content}}</p>
    </div>
  </div>

</template>

<style scoped>


</style>

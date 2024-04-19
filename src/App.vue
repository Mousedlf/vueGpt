<script setup lang="ts">

import axios from "axios";
import {ref} from "vue";
import {Message} from "@/interface/message";


const url = "http://127.0.0.1:11434/api/generate"
const prompt = ref("")
const messages = ref<Message[]>([])

async function submitPrompt(){

  messages.value.push({
    "content": prompt.value,
    "author": "user"
  });

  await axios.post(url, {
    "model": "mistral",
    "prompt": prompt.value,
    "stream": false
  }).then((response) => {
    console.log(response.data.response)

    messages.value.push({
      "content": response.data.response,
      "author": "ia"
    })
  })

  prompt.value = ""; //clear input
}


</script>

<template>
  <div class="p-5">

    <div class="mb-3">
      <form action="" method="" class="input-group" @submit.prevent="submitPrompt">
        <input type="text" v-model="prompt" class="form-control" >
        <button type="submit" class="btn btn-success">Send</button>
      </form>

    </div>
    <div v-for="message in messages">
      <div class="mb-2">
        <p class="pb-0 mb-0">{{message.author}} : </p>
        <p class="fw-bold">{{message.content}}</p>
      </div>
    </div>

  </div>

</template>

<style scoped>

</style>

<template>
  <div class="w-full h-full flex justify-center items-center">
    <div class="max-w-xs w-full flex flex-col">
      <div v-if="hasQuestion" data-qa="setup" class="w-3/4 p-4 rounded-2xl bg-teal-800 text-white self-start">
        {{ question }}
      </div>
      <div v-if="isShowAnswer" data-qa="delivery" class="mt-2 w-3/4 p-4 rounded-2xl bg-red-800 text-white self-end">
        {{ answer }}
      </div>
      <button
        v-if="isShowAnswer"
        role="button"
        class="mt-4 bg-green py-2 rounded-lg col-span-1 hover:opacity-90"
        @click="handleAnotherOne"
      >
        Another
      </button>
      <button
        v-else
        role="button"
        class="mt-4 bg-green py-2 rounded-lg col-span-1 hover:opacity-90"
        @click="handleShowAnswer"
      >
        Tell Me!
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, onMounted, computed } from 'vue'

const question = ref('')
const answer = ref('')
const isShowAnswer = ref(false)
const hasQuestion = computed(() => question.value !== '')

onMounted(() => {
  getQuestion()
})

const handleShowAnswer = () => {
  isShowAnswer.value = true
}

const handleAnotherOne = () => {
  isShowAnswer.value = false

  getQuestion()
}

const getQuestion = async () => {
  question.value = ''
  answer.value = ''

  try {
    const call = await fetch('https://v2.jokeapi.dev/joke/christmas')
    const result = await call.json()

    question.value = result.setup
    answer.value = result.delivery
  } catch (err) {
    alert(`Something went wrong: ${err}`)
  }
}
</script>

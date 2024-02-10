<script setup>
import { onMounted, ref } from "vue";
import useAPI from "@/composables/useAPI";
import { useRoute } from "vue-router"
import BaseTitle from "@/components/BaseTitle.vue";
import DifficultyChip from "@/components/DifficultyChip.vue";
import MainScore from "@/components/MainScore.vue";

const api = useAPI()
const question = ref(null)
const route = useRoute()
const answers = ref([])




onMounted(async () => {
  question.value = await api.getQuestion(route.params.id)

  answers.value.push({
    id: answers.value.length,
    correct: true,
    answer: question.value.correct_answer
  })

  question.value.incorrect_answers.map((wrong_answer) => {
    answers.value.push({
      id: answers.value.length,
      correct: false,
      answer: wrong_answer
    })
    
  })
  
  answers.value = shuffle(answers.value)
  console.log(answers.value)
})

const shuffle = (array) => {

  for (let i = array.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1)); //random float value between 0 and 1 [0,1) times i + 1
    [array[i], array[j]] = [array[j], array[i]];
  }

  return array;
}

</script>


<template>
  <div v-if="question" class="flex h-full w-full flex-col items-center gap-8 p-10">
    <BaseTitle>{{  question.category }} - <MainScore></MainScore></BaseTitle>
    
    <!-- {{ question.question }} -->
    <div v-html="question.question" class="text-center text-2xl font-bold"></div>
    <div class="grid w-full flex-grow grid-cols-2 gap-8">
      <div v-html="answer.answer" v-for="answer in answers" :key="answer.id" class="bg-green-500 flex items-center justify-center rounded-lg text-center text-4xl text-white py-10 px-2"></div>
    </div>
    <DifficultyChip :difficulty="question.difficulty"></DifficultyChip>
  </div>
   <div v-else class="loading">Loading...</div>

</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { useStore } from '@/store'

export default defineComponent({
  data() {
    return {
      tmpTitle: '',
      tmpTime: 1,
      tmpDescription: '',
      submittedOnce: false,
      settings: useStore()
    }
  },
  emits: ['addActivity'],
  methods: {
    handleSubmit(this: any) {
      // Should check for errors :)
      this.$emit('addActivity', this.tmpTitle, this.tmpTime, this.tmpDescription)
      this.tmpTitle = ''
      this.tmpTime = 1
      this.tmpDescription = ''

      this.submittedOnce = false
    }
  }
})
</script>

<template>
  <div class="rounded-b-lg p-12 bg-cartabinaria-light-blu">
    <form @submit.prevent="handleSubmit()">
      <div class="mt-5">
        <label for="title">Titolo: </label>
        <input
          v-model="tmpTitle"
          id="title"
          type="text"
          required
          class="rounded border p-2 bg-sky-100 text-black"
          :class="{
            'invalid:bg-red-100': submittedOnce,
            'invalid:border-red-200': submittedOnce,
            'invalid:border': submittedOnce
          }"
        />
      </div>

      <div class="mt-5">
        <label>Ore:</label>
        <div v-if="!settings.state.timeAsButton">
          <input
            v-model="tmpTime"
            type="number"
            min="1"
            class="rounded border p-2 bg-sky-100 text-black invalid:border-red-200 invalid:border invalid:bg-red-100"
          />
        </div>
        <div v-else class="text-yellow-500">
          <div class="flex justify-between gap-2">
            <button
              type="button"
              v-for="n in Array.from(Array(10), (_, i) => i + 1)"
              class="border w-full hover:bg-blue-500 rounded p-2"
              :class="{ 'bg-blue-500': tmpTime == n }"
              @click="tmpTime = n"
            >
              {{ n }}
            </button>
          </div>
        </div>
      </div>

      <div class="mt-5">
        <label for="description">Descrizione:</label>
        <textarea
          v-model="tmpDescription"
          placeholder="Add description..."
          required
          class="block h-32 w-full rounded border p-2 outline-none bg-sky-100 text-black"
          :class="{
            'invalid:bg-red-100': submittedOnce,
            'invalid:border-red-200': submittedOnce,
            'invalid:border': submittedOnce
          }"
        ></textarea>
      </div>

      <div class="flex justify-center">
        <button
          class="m-5 w-20 rounded bg-blue-400 p-2 border-0 text-gray-50 hover:bg-blue-500 hover:text-white"
          @click="submittedOnce = true"
        >
          Add
        </button>
      </div>
    </form>
  </div>
</template>

<style scoped>
input {
  /*border-bottom: thin solid gray;*/
  display: block;
  width: 100%;
  outline: none;
}

label {
  display: block;
  font-weight: bold;
  margin-bottom: 10px;
}
</style>

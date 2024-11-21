<template>
  <q-page class="flex flex-center text-white" v-touch-pan.vertical.prevent.mouse="handlePan">
    <div class="row">
      <q-input
        v-model="data.name"
        filled
        label="Filled"
        input-class="text-center text-h5 text-white"
        color="teal"
      />
    </div>
    <div class="row full-width items-center">
      <div class="col text-center">
        <q-btn
          @click="decreaseCounter"
          v-touch-repeat:300:300:300:50.mouse="decreaseCounter"
          round
          size="xl"
          icon="remove" />
      </div>
      <div class="col text-center text-h2">
        {{ data.counter }}
      </div>
      <div class="col text-center">
        <q-btn
          @click="increaseCounter"
          v-touch-repeat:300:300:300:50.mouse="increaseCounter"
          round
          size="xl"
          icon="add" />
      </div>
    </div>
    <div class="row">
      <q-btn
          v-touch-repeat:300:300:50.mouse="resetCounter"
          round
          size="xl"
          icon="restart_alt" />
    </div>
  </q-page>
</template>

<style scoped>
.q-page {
  max-width: 700px;
  margin: 0 auto;
}
</style>

<script setup>
import { reactive, watch } from "vue";
import { useQuasar } from 'quasar'

const $q = useQuasar()
const data = reactive({
  counter: 0,
  name: ''
})

const savedData = $q.localStorage.getItem('data')
if (savedData) Object.assign(data, savedData)

watch(data, value => {
  $q.localStorage.set('data', value);
})

const increaseCounter = () => {
  data.counter++
}

const decreaseCounter = () => {
  if (data.counter > 0) data.counter--
}

const resetCounter = () => {
  data.counter = 0
}

const handlePan = (event) => {
  event.delta.y
  if (event.delta.y < 0) {
    increaseCounter()
  } else {
    decreaseCounter()
  }
}
</script>

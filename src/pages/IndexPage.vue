<template>
  <q-page class="flex flex-center text-white" 
  v-touch-pan.vertical.prevent.mouse="handlePan">
    <div class="row">
       <q-input v-model="data.name" filled placeholder="Counter" input-class="text-center text-h5 text-white" color="teal" />
    </div>
   <div class="row full-width items-center">
   <div class="col text-center">
    <q-btn @click="decreaseCounter"
    v-touch-repeat:300:300:300:300:50.mouse.enter.space="decreaseCounter"
    round icon="remove" size="xl" />
   </div>
   <div class="col text-center text-h2">{{ data.counter }}</div>
   <div class="col text-center">
    <q-btn @click="increaseCounter" 
    v-touch-repeat:300:300:300:300:50.mouse.enter.space="increaseCounter"
    round icon="add" size="xl" />
   </div>
   </div>
   <div class="row">
     <q-btn @click="resetCounter" round icon="restart_alt" size="xl" />

   </div>
  </q-page>
</template>

<style scoped>
.q-page {
  max-width: 44rem;   /* Use the primary color defined in quasar.variables.scss */
margin: 0 auto;
  
}
</style>
<script setup>

//imports

import { reactive, watch } from 'vue';
import { useQuasar } from 'quasar';


/*
data
 */

 const $q = useQuasar();

const data = reactive({
  counter: 0,
  name: '',
});

const savedData = $q.localStorage.getItem('data');
if (savedData) {
  Object.assign(data, savedData);
} 

watch(data, value => {
  $q.localStorage.set('data', value);
});
/*
counter methods
 */

const increaseCounter = () => {
  data.counter++;
};

const decreaseCounter = () => {
  if (data.counter > 0){
    data.counter--;
  } 
  
};

const resetCounter = () => {
  data.counter = 0;
};

/*
touch pan handler
 */
const handlePan = (event) => {
  if (event.delta.y < 0) {
    increaseCounter();
  } else {
    decreaseCounter();
  }
}
</script>

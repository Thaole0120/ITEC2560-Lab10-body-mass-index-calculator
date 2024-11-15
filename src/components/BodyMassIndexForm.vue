<script setup>
import { ref, defineEmits, watch, defineProps } from 'vue'


const props = defineProps({
    isMetricSelected: Boolean // Pro to know whether to use metric or American units
})

//Define the event that will be emitted to the App.vue
const emit = defineEmits () 

//Reactive variables for height and weight
const height = ref(null)
const weight = ref(null)

const heightMax = props.isMetricSelected ? 3 : 108 //Max height in meters or inches ( under 3 meters tall or 108 inches)
const weightMax = props.isMetricSelected ? 300 : 1000 // Max weight in kilograms or pounds ( under 300 kg or 1000lbs)


//Function to emit the stats-entered event with two variables height and weight 
const emitStats = () => {
  if (height.value && 
  weight.value && 
  !isNaN(height.value) &&
  !isNaN(weight.value) &&
  height.value > 0 && 
  weight.value > 0 &&
  height.value <= heightMax && 
  weight.value <= weightMax) {
    emit ('stats-entered', {height: height.value, weight: weight.value })
  }else{
    alert (' Please enter a valid height and weight value')
  }
}

// Reset height and weight if the unit changes
watch(() => props.isMetricSelected, () => {
  height.value = null
  weight.value = null
})

</script>

<template>
  <div class="bmi-form">
    <h2>Enter your height and weight</h2>

    <!-- Height input-->
     <label for="height"> {{ props.isMetricSelected ? 'Height in meters' : 'Height in inches'  }}</label>
     <input type="number" id="height" v-model.number="height" :min="1" :max="heightMax"/>

    <!-- Weight input-->
     <label for="weight"> {{ props.isMetricSelected ? 'Weight in kilograms' : 'Weight in pounds' }}</label>
     <input type="number" id="weight" v-model.number="weight" :min="1" :max="weightMax"> 

     <!-- Calculate button -->
      <button type="button" @click="emitStats">Calculate</button>

  </div>

</template>

<style scoped>

.bmi-form {
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  max-width: 400px;
  margin: auto;
}

h2 {
  text-align: center;
}

label {
  display: block;
  margin-top: 10px;
}

input {
  width: 100%;
  padding: 8px;
  margin-top: 5px;
  border-radius: 4px;
  border: 1px solid #ccc;
}

button {
  width: 100%;
  padding: 10px;
  background-color: #007bff;
  color: white;
  border: none;
  border-radius: 4px;
  margin-top: 15px;
  cursor: pointer;
}

button:hover {
  background-color: #0056b3;
}
</style>

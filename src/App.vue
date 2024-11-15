<script setup>
import { ref } from 'vue'
import BodyMassIndexForm from './components/BodyMassIndexForm.vue'

// Reactive variable to store the calculated BMI
const bmi = ref(null)
const isMetricSelected = ref(true)

// Function to handle the stats-entered event
const calculateBMI = (data) => {
  const { height, weight } = data

  if (isMetricSelected.value) {
  // Metric units: height in meters, weight in kilograms
  if (height > 0 && weight > 0) {
    // BMI calculation: weight / (height * height)
    bmi.value = weight / (height * height) 
  } 
} else {
    // American units: height in inches, weight in pounds
    if (height > 0 && weight > 0) {
      bmi.value = (weight / (height * height)) * 730 
    }
  }
}

//Function to reset all values when the units change
const resetData = () => {
  bmi.value = null // Reset BMI when units are changed
 }
</script>

<template>
  <div>
    <h1> Body Mass Index Calculator</h1>

    <!-- Checkbox for selecting unit system -->
    <div>
      <label>
        <input type="checkbox" v-model="isMetricSelected" @change="resetData" />
        Use metric units (height in meters, weight in kilograms)
      </label>
    </div>
    <!-- Include the BodyMassIndexForm component and listen for the 'stats-entered' event -->
     <BodyMassIndexForm :isMetricSelected="isMetricSelected" @stats-entered="calculateBMI" />
    
    <!-- Display the BMI result if calculated -->
    <p v-if="bmi !== null">BMI is: {{ bmi.toFixed(2) }}</p>
    <p v-else> No valid data yet</p>
  </div>

</template>

<style scoped>
h1 {
  text-align: center;
  color: #2c3e50;
  margin-bottom: 20px;
}

p {
  font-size: 18px;
  font-weight: bold;
  text-align: center;
  color: lightseagreen ;
}

body {
  font-family: 'Arial', sans-serif;
  text-align: center;
  padding: 20px;
}
</style>

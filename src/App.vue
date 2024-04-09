<template>
  
  <div class="shafts">
    <h1>Эмулятор Лифта</h1>
    <h2>Савельев Александр</h2>
    <Shaft :numberOfFloors="numberOfFloors" />
  </div>
</template>

<script>
import Shaft from './Components/ElevatorShaft.vue';

export default {
  components: {
    Shaft
  },
  data() {
    return {  
      numberOfFloors: 5      
    };
  }
};
</script>

<style>
.shafts {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 20px;
  align-items: center;
  padding-bottom: 5vh;
}
</style>

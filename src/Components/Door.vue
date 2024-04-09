<template>
    <div class="elevator-door" :class="{ 'resting-animation': resting }" :style="{ bottom: shaftMeasures.bottomPosition, height: shaftMeasures.doorHeight }">
      <div class="elevator-display" :class="{ 'working': isWorking, 'error': !isWorking }">
        <span v-show="isWorking && aimFloor !== currentFloor">{{ direction }}</span>
        <span>{{ aimFloor }}</span>
        <div class="rest-indicator" v-if="resting"></div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      aimFloor: {
        type: Number,
        required: true
      },
      currentFloor: {
        type: Number,
        required: true
      },
      direction: {
        type: String,
        required: true
      },
      numberOfFloors: {
        type: Number,
        required: true
      },
      resting: {
        type: Boolean,
        default: false
      },
      isWorking: {
        type: Boolean,
        default: true
      }
    },
    computed: {
      shaftMeasures() {
        const floorHeight = 100 / this.numberOfFloors;
        const doorHeight = floorHeight - 0.75;
        const bottomPosition = (this.currentFloor - 1) * floorHeight;
        return { bottomPosition: `${bottomPosition}%`, doorHeight: `${doorHeight}%` };
      }
    }
  };
  </script>
  
  <style scoped>
  .elevator-door {
    width: 80%;
    background-color: #c0c0c0;
    position: absolute;
    border: 2px solid rgb(21, 51, 57);
    display: flex;
    align-items: flex-start;
    justify-content: center;
    transition: bottom 0.5s cubic-bezier(0.645, 0.045, 0.355, 1); /* Скорость анимации с кривой Безье */
  }
  
  .elevator-display {
    width: 60%;
    height: 20px;
    margin-top: 5px;
    background-color: #fff;
    border: 2px solid #000;
    display: flex;
    gap: 6px;
    align-items: center;
    justify-content: center;
    font-size: 16px;
  }
  
  .elevator-display.working {
    background-color: #6eeb6e;
  }
  
  .elevator-display.error {
    background-color: #fd9c9c;
  }
  
  .rest-indicator {
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background-color: rgba(252, 35, 35, 0.482);
    animation: blink 0.5s infinite alternate;
  }
  
  .resting-animation { 
    transition: bottom 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
  }
  
  @keyframes blink {
    from {
      opacity: 1;
    }
    to {
      opacity: 0;
    }
  }
  </style>
  
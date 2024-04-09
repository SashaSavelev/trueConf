<template>
    <div class="shaft">
        <div class="divider" v-for="floor in reversedFloors" :key="floor">
            <button class="floor" @click="goToFloor(floor)">{{ floor }}</button>
        </div>
        <Door :isWorking="isWorking" :aimFloor="aimFloor" :currentFloor="currentFloor" :numberOfFloors="numberOfFloors"
            :direction="direction" :resting="resting" />
    </div>
</template>

<script>
import Door from './Door.vue';

export default {
    name: 'shaft',
    components: {
        Door
    },
    props: {
        numberOfFloors: {
            type: Number,
            required: true
        }
    },
    data() {
        return {
            direction: '',
            aimFloor: 1,
            currentFloor: 1,
            moving: false,
            resting: false,
            isWorking: true,
            commands: [] 
        };
    },
    computed: {
        reversedFloors() {
            return [...Array(this.numberOfFloors).keys()].reverse().map(floor => floor + 1);
        }
    },
    methods: {
        async goToFloor(floor) {
               if (this.currentFloor == floor) {
                return;
               } 
            if (this.moving) {

                if (this.commands.includes(floor) || this.aimFloor == floor) {
                    this.isWorking = false;
                    setTimeout(() => {
                        this.isWorking = true;
                    }, 1000);
                    return;
                }
                this.addCommand(floor);
                return;
            }
            this.moving = true;
            this.direction = floor > this.currentFloor ? '↑' : '↓';
            this.aimFloor = floor;
            const delay = 1000; 
            const floorsToMove = Math.abs(this.currentFloor - floor);
            const directionMultiplier = floor > this.currentFloor ? 1 : -1;
            for (let i = 0; i < floorsToMove; i++) {
                await this.moveOneFloor(delay);
                this.currentFloor += directionMultiplier;
            }
            this.resting = true;
            setTimeout(() => {
                this.moving = false; 
                this.resting = false;
                this.executeCommands(); 
            }, 3300);
        },
        moveOneFloor(delay) {
            return new Promise(resolve => {
                setTimeout(() => {
                    resolve();
                }, delay);
            });
        },
        addCommand(floor) {
            this.commands.push(floor);
        },
        async executeCommands() {
            if (this.commands.length === 0) return;          
            this.commands = this.commands.filter((item, index, array) => array.indexOf(item) === index);
            const nextFloor = this.commands.shift(); 
            await this.goToFloor(nextFloor); 
        }
    }
};
</script>

<style>
.shaft {
    background-color: rgb(52, 100, 100);
    width: 200px;
    height: 80%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    position: relative;
}

.divider {
    height: 100%;
    width: 100%;
    border-bottom: 2px solid black;
    position: relative;
}

.floor {
    z-index: 1000;
    position: absolute;
    right: 0;
    background-color: #9effa1;
    /* Зеленый цвет */
    border: none;
    color: rgb(0, 0, 0);
    padding: 5px 16px;
    text-align: center;
    text-decoration: none;
    display: inline-block;
    font-size: 16px;
    margin: 4px 2px;
    cursor: pointer;
    border-radius: 19px;
}
</style>
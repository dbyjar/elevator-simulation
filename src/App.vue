<script setup>
import { onMounted, ref } from "vue"

const informationMsg = ref("");

const guestName = ref("")
const hasGuestName = ref(false)

const errorMsg = ref("")
const error = ref(false)

onMounted(() => {
  setError(false, "")
  reset();
})

const setError = (isTrue, msg) => {
  error.value = isTrue;
  errorMsg.value = msg;
}

const reset = () => {
  guestName.value = "";
  isPickedUp.value = true;
  hasGuestName.value = false;
  currentFloor.value = Math.floor(Math.random() * 10) + 1;
  informationMsg.value = "Welcome to the Elevator by Jar." + '\n' + "Please input your name first to continue use this elevator.";
}

const onSubmitClick = () => {
  setError(false, "")

  if (guestName.value.length >= 5) {
    if (!hasGuestName.value) {
      hasGuestName.value = true;
      informationMsg.value = `Hi ${guestName.value}, click your pickup floor button`
    } else {
      reset();
    }
  } else {
    setError(true, "input at least 5 characters")
  }
}

const currentFloor = ref(1)
const isPickedUp = ref(true)

const onTargetFloorCLick = (number) => {
  setError(false, "")

  if (currentFloor.value === number) {
    if (!isPickedUp.value) {
      setError(true, "you don't need to use the elevator, because you are on the same floor")
      return;
    }
  }

  const rangeNumberFloor = Math.abs(currentFloor.value - number)
  toTargetFloor(rangeNumberFloor, currentFloor.value < number)
}

const disableNumberButton = ref(false)

const toTargetFloor = (value, isUp) => {
    disableNumberButton.value = true;

    setTimeout(() => {
      if (value >= 1) {
        isUp
          ? currentFloor.value += 1
          : currentFloor.value -= 1

        toTargetFloor((value - 1), isUp)
      } else {
        informationMsg.value = isPickedUp.value ? "click your destination floor button" : "you have arrived :)"
        disableNumberButton.value = false;
        isPickedUp.value = false;
      }
    }, 1000);
}
</script>

<template>
  <div>
    <div class="wrapper">
      <div style="margin: 20px 0">
        <div class="information-msg">
          {{ informationMsg }}
        </div>
        <form @submit.prevent="onSubmitClick">
          <input type="text" v-model="guestName" placeholder="input your name" v-if="!hasGuestName">
          <p class="error-text" v-if="error">
            {{ errorMsg }}
          </p>
        </form>
      </div>
      <small style="text-transform: capitalize;">
        elevator currently located at:
      </small>
      <div class="current-floor-number">{{ currentFloor }}</div>
      <div class="button-elevator-number">
        <div class="row">
          <button type="button" class="number" :disabled="!hasGuestName || disableNumberButton" @click.prevent="onTargetFloorCLick(1)">1</button>
          <button type="button" class="number" :disabled="!hasGuestName || disableNumberButton" @click.prevent="onTargetFloorCLick(2)">2</button>
          <button type="button" class="number" :disabled="!hasGuestName || disableNumberButton" @click.prevent="onTargetFloorCLick(3)">3</button>
        </div>
        <div class="row">
          <button type="button" class="number" :disabled="!hasGuestName || disableNumberButton" @click.prevent="onTargetFloorCLick(4)">4</button>
          <button type="button" class="number" :disabled="!hasGuestName || disableNumberButton" @click.prevent="onTargetFloorCLick(5)">5</button>
          <button type="button" class="number" :disabled="!hasGuestName || disableNumberButton" @click.prevent="onTargetFloorCLick(6)">6</button>
        </div>
        <div class="row">
          <button type="button" class="number" :disabled="!hasGuestName || disableNumberButton" @click.prevent="onTargetFloorCLick(7)">7</button>
          <button type="button" class="number" :disabled="!hasGuestName || disableNumberButton" @click.prevent="onTargetFloorCLick(8)">8</button>
          <button type="button" class="number" :disabled="!hasGuestName || disableNumberButton" @click.prevent="onTargetFloorCLick(9)">9</button>
        </div>
        <div class="row">
          <button type="button" class="number" :disabled="!hasGuestName || disableNumberButton" @click.prevent="onTargetFloorCLick(10)">10</button>
          <button type="button" class="submit" :disabled="!guestName.length || disableNumberButton" @click.prevent="onSubmitClick()">
            {{ !hasGuestName ? 'SUBMIT' : 'RESET' }}
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.wrapper {
  width: 200px;
  position: relative;
  padding: 20px;
}

.information-msg {
  font-size: 12px;
  margin: 10px 0;
  background: rgb(53, 92, 198);
  padding: 20px;
  border-radius: 5px;
}

input {
  width: -webkit-fill-available;
}

.error-text {
  color: red;
  font-size: 12px;
  margin: 5px 0;
}

.current-floor-number {
  background: crimson;
  text-align: center;
  padding: 10px;
  border-radius: 5px;
  margin-top: 5px;
  margin-bottom: 20px;
}

.row {
  display: flex;
  justify-content: space-between;
  width: 100%;
  margin-bottom: 10px;
}

.number {
  width: 60px;
  height: 60px;
  border-radius: 100%;
}

button.submit {
    border-radius: 35px;
}
</style>

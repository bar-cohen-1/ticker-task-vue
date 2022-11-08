<template>
  <main>
    <div class="ticker">
      <span>{{ count }}</span>
    </div>
    <div class="actions">
      <input v-model="limit" type="number" />
      <button class="reset-button" @click="resetCount">Reset</button>
      <button class="activation-button" @click="activateCount">
        {{ activationButtonText }}
      </button>
    </div>
  </main>
</template>

<script setup>
import { ref, watch, computed, onMounted, onBeforeUnmount } from "vue";

// State
const interval = ref(null);
const count = ref(0);
const limit = ref(60);

// Reset
const resetCount = () => {
  count.value = 0;
};

// Activate button properties
const activationButtonText = computed(() => {
  return interval.value ? "Pause" : "Resume";
});
const activateCount = computed(() => {
  return interval.value ? stopCount : startCount;
});

// Activate methods
const startCount = () => {
  interval.value = setInterval(() => {
    count.value++;
  }, 1000);
};
const stopCount = () => {
  clearInterval(interval.value);
  interval.value = null;
};

// Limit effect
watch([count, limit], ([newCount, newLimit]) => {
  if (newCount === newLimit) {
    resetCount();
  }
});

// Lifecycle methods
onMounted(() => {
  startCount();
});
onBeforeUnmount(() => {
  stopCount();
});
</script>

<style scoped>
main {
  width: 400px;
  height: 200px;
  padding: 0 1em;
  border: 2px solid darkblue;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.ticker {
  width: 6em;
  height: 6em;
  border-radius: 50%;
  border: 2px solid blue;
  background-color: yellow;
  display: flex;
  justify-content: center;
  align-items: center;
}

.ticker span {
  font-size: 2em;
}

.actions {
  display: flex;
  flex-direction: column;
  gap: 0.5em;
}

.actions button,
.actions input {
  width: 6em;
  padding: 0.5em 1em;
}

.actions button {
  color: white;
  cursor: pointer;
}

.reset-button {
  background-color: green;
}

.activation-button {
  background-color: red;
}
</style>

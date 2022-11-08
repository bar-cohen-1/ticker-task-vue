<template>
  <main>
    <div class="ticker">
      <span>{{ count }}</span>
    </div>
    <div class="actions">
      <button class="reset-button" @click="resetCount">Reset</button>
      <button class="activation-button" @click="activateCount">
        {{ activationButtonText }}
      </button>
    </div>
  </main>
</template>

<script>
import { computed, onMounted, onBeforeUnmount, ref } from "vue";

export default {
  setup() {
    const interval = ref(null);
    const count = ref(0);

    const startCount = () => {
      interval.value = setInterval(() => {
        count.value++;
      }, 1000);
    };
    const stopCount = () => {
      clearInterval(interval.value);
      interval.value = null;
    };
    const resetCount = () => {
      count.value = 0;
    };

    const activationButtonText = computed(() => {
      return interval.value ? "Pause" : "Resume";
    });
    const activateCount = computed(() => {
      return interval.value ? stopCount : startCount;
    });

    onMounted(() => {
      startCount();
    });
    onBeforeUnmount(() => {
      stopCount();
    });

    return { count, activationButtonText, activateCount, resetCount };
  },
};
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

.actions button {
  width: 6em;
  padding: 0.5em 1em;
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

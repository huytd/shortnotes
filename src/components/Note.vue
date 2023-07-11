<script setup>
import { computed, defineEmits, defineProps, ref } from "vue";
import marked from "marked";

const LIFETIME_BEGIN = 60 * 5;
const LIFETIME_END = 0;

const props = defineProps({
  note: String,
});

const dateFormatter = new Intl.DateTimeFormat("en-US", {
  hour: "numeric",
  minute: "numeric",
  second: "numeric",
});
const createdAt = dateFormatter.format(Date.now());
const self = ref(null);
const emit = defineEmits(["destroy"]);

const htmlContent = marked.parse(props.note);
const timeLeft = ref(LIFETIME_BEGIN);
const opacity = computed(() => timeLeft.value / LIFETIME_BEGIN);
const progress = computed(() => (timeLeft.value / LIFETIME_BEGIN) * 100 + "%");

let countDownTimer = setInterval(() => {
  if (timeLeft.value > LIFETIME_END) {
    timeLeft.value -= 1;
  } else {
    clearInterval(countDownTimer);
    countDownTimer = null;
    destroy();
  }
}, 1000);

const destroy = () => {
  emit("destroy", self);
};

const renew = () => {
  timeLeft.value = LIFETIME_BEGIN;
  navigator.clipboard.writeText(props.note);
};
</script>

<template>
  <div
    class="note-entry"
    ref="self"
    :style="{ opacity: opacity }"
    @click="renew"
    @dblclick="destroy"
  >
    <div class="time">{{ createdAt }}</div>
    <div class="content">
      <div v-html="htmlContent"></div>
      <div class="entry-footer">
        <div class="health-bar">
          <div class="progress" :style="{ width: progress }"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.note-entry {
  transition: all 0.5s;
  display: flex;
}

.time {
  color: var(--primary-ui-color);
  width: 160px;
  flex-shrink: 0;
}

.content {
  padding: 0;
  margin: 0;
  font-family: inherit;
}

.entry-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 20px;
  margin-bottom: 30px;
}

.health-bar {
  display: block;
  clear: both;
  width: 50px;
  height: 4px;
  border: 1.5px solid var(--primary-ui-color);
  border-radius: 3px;
  position: relative;
}

.health-bar .progress {
  display: block;
  position: absolute;
  top: 0;
  left: 0;
  background: var(--primary-ui-color);
  width: 100%;
  height: 100%;
}
</style>

<style>
.content *:first-child {
  margin-top: 0;
}
</style>

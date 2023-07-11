<script setup>
import { ref } from "vue";
import Note from "./components/Note.vue";
import { mount } from "mount-vue-component";

const INTRODUCTION_NOTE = `# Welcome to ShortNotes!

## What is this app?

ShortNotes is a revolutionary note-taking app designed to help you declutter your mind and optimize your cognitive abilities. It leverages the concept of Short-Term Memory (STM) to enhance your thinking, problem-solving, and overall productivity.

## Why use ShortNotes?

Short-Term Memory (STM) is the mental workspace where your brain holds a limited amount of information in an active and readily available state for a short period. Research suggests that STM has a capacity of approximately 7 items for around 20 seconds (individual experiences may vary).

By offloading unimportant thoughts, ideas, and reminders to ShortNotes, you create more space in your STM, allowing you to focus on critical tasks and process information more effectively. Think of it as an external extension of your memory, ready to assist you whenever you need it.

## How does it work?

ShortNotes embraces the nature of STM by making your notes fade away automatically after 5 minutes. Each note acts as a temporary placeholder for your thoughts, encouraging you to let go of non-essential information. However, if you need more time with a particular note, you can easily extend its lifespan by clicking on it for an additional 5 minutes.

## Getting started with ShortNotes

1. Open ShortNotes whenever you experience a context switch or feel overwhelmed by the amount of information in your mind.
2. Quickly jot down your thoughts, ideas, or reminders without worrying about them cluttering your mind.
3. Carry on with your tasks, knowing that your notes will be available when you need them.
4. When you revisit ShortNotes, you can choose to retrieve the notes that are still relevant or let them naturally fade away, freeing up your mental resources.
5. Repeat this process whenever you need to declutter your mind and optimize your cognitive performance.

## Usage tips

1. Click on a note to add an extra 5 minutes to its lifetime
2. Double click on a note to copy its content to clipboard

Read more: [https://en.wikipedia.org/wiki/Short-term_memory](https://en.wikipedia.org/wiki/Short-term_memory)
`;

const noteList = ref(null);
const noteInput = ref(null);

const destroyNote = (ref) => {
  ref.value.remove();
};

const createNote = (event) => {
  if (!event.shiftKey) {
    const noteContent = noteInput.value.value;

    const { el } = mount(Note, {
      props: {
        note: noteContent,
        onDestroy: destroyNote,
      },
    });

    noteList.value.appendChild(el);

    event.preventDefault();
    noteInput.value.value = "";

    el.scrollIntoView();
  }
};
</script>

<template>
  <div class="note-list" ref="noteList">
    <Note :note="INTRODUCTION_NOTE" @destroy="destroyNote" />
  </div>
  <textarea autofocus ref="noteInput" @keypress.enter="createNote"></textarea>
  <div class="help-text">
    Press ↵ Enter to save or ⇧ Shift + ↵ Enter to add new line
  </div>
</template>

<style>
:root {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica,
    Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
  font-size: 16px;

  --primary-content-color: #111;
  --primary-ui-color: #999;

  color: var(--primary-content-color);
}

body {
  margin: 0;
  padding: 0;
}

#app {
  display: flex;
  flex-direction: column;
  height: calc(100vh - 40px);
  padding: 20px;
  position: relative;
}
</style>

<style scoped>
.note-list {
  flex: 1;
  overflow-y: auto;
}

textarea {
  height: 100px;
  resize: none;
  border: 1.5px solid var(--primary-ui-color);
  color: var(--primary-content-color);
  border-radius: 3px;
  padding: 10px;
  font-family: inherit;
  outline: none;
}

.help-text {
  font-size: 14px;
  position: absolute;
  right: 28px;
  bottom: 28px;
  color: var(--primary-ui-color);
}
</style>
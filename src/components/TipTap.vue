<script setup>
import { EditorContent } from '@tiptap/vue-3';

const props = defineProps(['editor']);
</script>

<template>
  <editor-content :editor="props.editor" />
</template>

<style scoped>
:deep(.tiptap) {
  outline: none !important;
}
:deep(.tiptap h1) {
  font-size: 3.6rem;
}
:deep(.tiptap h2) {
  font-size: 2.8rem;
}
:deep(.tiptap h3) {
  font-size: 2.2rem;
}

:deep(.tiptap blockquote) {
  padding-left: 1rem;
  border-left: 3px solid #ccc;
}

:deep(.tiptap p) {
  font-size: 1.6rem;
}
:deep(.tiptap p.is-empty::before) {
  color: #adb5bd;
  content: attr(data-placeholder);
  float: left;
  height: 0;
  pointer-events: none;
}

:deep(.tiptap mark) {
  padding: 0 0.3rem;
  background-color: #e0f2fe;
  border-radius: 0.5rem;
}

:deep(.tiptap a) {
  color: #007bff;
  cursor: pointer;
}

:deep(.tiptap p code) {
  font-size: 1.3rem;
  padding: 0.5rem;
  color: #555;
  background-color: #f5f5f5;
  border-radius: 0.5rem;
}
:deep(.tiptap pre code) {
  display: block;
  width: 100%;
  font-size: 1.3rem;
  padding: 2rem;
  color: #555;
  background-color: #f5f5f5;
  border: 1px solid #ccc;
  border-radius: 0.5rem;
}

:deep(.tiptap ol) {
  font-size: 1.6rem;
  padding-left: 1.5rem;
}
:deep(.tiptap ul) {
  list-style-type: '- ';
  font-size: 1.6rem;
  padding-left: 1rem;
}
:deep(.tiptap ul[data-type='taskList']) {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  padding-left: 0;
}
:deep(.tiptap ul[data-type='taskList'] li) {
  display: flex;
  gap: 0.5rem;
}
:deep(.tiptap ul[data-type='taskList'] li label) {
  padding-top: 0.05rem;
}
</style>

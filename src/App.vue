<script setup>
import { onBeforeUnmount } from 'vue';

import { useEditor } from '@tiptap/vue-3';
import StarterKit from '@tiptap/starter-kit';
import { Placeholder } from '@tiptap/extensions';
import { TaskList, TaskItem } from '@tiptap/extension-list';
import Highlight from '@tiptap/extension-highlight';
import TextAlign from '@tiptap/extension-text-align';
import CodeBlockLowlight from '@tiptap/extension-code-block-lowlight';
import { createLowlight, common } from 'lowlight';

import TopTool from './components/TopTool.vue';
import TipTap from './components/TipTap.vue';

const editor = useEditor({
  // content: "<p>I'm running Tiptap with Vue.js. 🎉</p>",
  extensions: [
    StarterKit.configure({ codeBlock: false }),
    Placeholder.configure({
      placeholder: 'Enter text...',
    }),
    TaskList,
    TaskItem.configure({
      nested: true,
    }),
    Highlight,
    TextAlign.configure({
      types: ['heading', 'paragraph'],
    }),
    CodeBlockLowlight.configure({
      lowlight: createLowlight(common),
      defaultLanguage: 'plaintext', // 可選
      languageClassPrefix: 'language-', // 可選
    }),
  ],
});

onBeforeUnmount(() => {
  editor.value.destroy();
});

const getData = (fn) => {
  console.log(editor.value[fn]());
};
</script>

<template>
  <template v-if="editor">
    <section>
      <TopTool :editor="editor" />
      <article>
        <TipTap :editor="editor" />
      </article>
      <footer>
        <button type="button" @click="getData('getJSON')">get JSON</button>
        <button type="button" @click="getData('getHTML')">get HTML</button>
      </footer>
    </section>
  </template>
</template>

<style scoped>
section {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 100%;
  width: 100%;
  padding: 10rem 8rem;
  font-size: 1.6rem;
}

article {
  flex-grow: 1;
  width: 100%;
  padding: 6rem 10rem;
  overflow-y: auto;
}

article > :deep(div) {
  height: 100%;
  width: 100%;
}

footer {
  display: flex;
  gap: 1rem;
}
footer button {
  padding: 1rem 1.5rem;
  background-color: #eee;
  border: none;
  border-radius: 1rem;
  cursor: pointer;
  transition: all 0.3s ease;
}
footer button:hover {
  background-color: #ddd;
}
</style>

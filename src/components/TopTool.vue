<script setup>
import { ref, nextTick } from 'vue';
import { onClickOutside } from '@vueuse/core';
import {
  Heading1,
  Heading2,
  Heading3,
  List,
  ListOrdered,
  ListTodo,
  Bold,
  Italic,
  Strikethrough,
  Underline,
  Highlighter,
  Link,
  Send,
  Trash,
  CodeXml,
  SquareCode,
  Quote,
  AlignLeft,
  AlignCenter,
  AlignRight,
  AlignJustify,
} from 'lucide-vue-next';

const props = defineProps({
  editor: {
    type: Object,
    required: true,
  },
  size: {
    type: String,
    default: '2rem',
  },
});

const toggleActive = (type, data) => {
  const method = 'toggle' + type.charAt(0).toUpperCase() + type.slice(1);
  props.editor.chain().focus()[method](data).run();
};

const popupRef = ref(null);
const popupShow = ref(false);
const linkRef = ref(null);
const linkUrl = ref('');

const togglePopup = () => {
  popupShow.value = !popupShow.value;
  if (popupShow.value) {
    nextTick(() => {
      linkUrl.value = props.editor.getAttributes('link').href;
      linkRef.value.focus();
    });
  }
  linkUrl.value = '';
};
onClickOutside(popupRef, () => {
  popupShow.value = false;
  linkUrl.value = '';
});
const addLink = () => {
  if (!linkUrl.value) return;
  props.editor
    .chain()
    .focus()
    .extendMarkRange('link')
    .setLink({ href: linkUrl.value })
    .run();
  togglePopup();
};
const removeLink = () => {
  props.editor.chain().focus().extendMarkRange('link').unsetLink().run();
  togglePopup();
};
</script>

<template>
  <header>
    <span class="space"></span>

    <!-- Heading -->
    <button
      :class="{ active: props.editor.isActive('heading', { level: 1 }) }"
      @click="toggleActive('heading', { level: 1 })"
    >
      <Heading1 :size="props.size" />
    </button>
    <button
      :class="{ active: props.editor.isActive('heading', { level: 2 }) }"
      @click="toggleActive('heading', { level: 2 })"
    >
      <Heading2 :size="props.size" />
    </button>
    <button
      :class="{ active: props.editor.isActive('heading', { level: 3 }) }"
      @click="toggleActive('heading', { level: 3 })"
    >
      <Heading3 :size="props.size" />
    </button>

    <!-- List -->
    <button
      :class="{ active: props.editor.isActive('bulletList') }"
      @click="toggleActive('bulletList')"
    >
      <List :size="props.size" />
    </button>
    <button
      :class="{ active: props.editor.isActive('orderedList') }"
      @click="toggleActive('orderedList')"
    >
      <ListOrdered :size="props.size" />
    </button>
    <button
      :class="{ active: props.editor.isActive('taskList') }"
      @click="toggleActive('taskList')"
    >
      <ListTodo :size="props.size" />
    </button>

    <span class="separator"></span>

    <!-- Bold -->
    <button
      :class="{ active: props.editor.isActive('bold') }"
      @click="toggleActive('bold')"
    >
      <Bold :size="props.size" :stroke-width="2.5" />
    </button>

    <!-- Italic -->
    <button
      :class="{ active: props.editor.isActive('italic') }"
      @click="toggleActive('italic')"
    >
      <Italic :size="props.size" />
    </button>

    <!-- Strike -->
    <button
      :class="{ active: props.editor.isActive('strike') }"
      @click="toggleActive('strike')"
    >
      <Strikethrough :size="props.size" />
    </button>

    <!-- Underline -->
    <button
      :class="{ active: props.editor.isActive('underline') }"
      @click="toggleActive('underline')"
    >
      <Underline :size="props.size" />
    </button>

    <!-- Highlight -->
    <button
      :class="{ active: props.editor.isActive('highlight') }"
      @click="toggleActive('highlight')"
    >
      <Highlighter :size="props.size" />
    </button>

    <!-- Link -->
    <div class="container">
      <button
        :class="{ active: props.editor.isActive('link') }"
        @click="togglePopup"
      >
        <Link :size="props.size" />
      </button>
      <div v-if="popupShow" ref="popupRef" class="popup">
        <input
          ref="linkRef"
          type="text"
          placeholder="Paste a link..."
          v-model="linkUrl"
        />
        <button @click="addLink"><Send size="1.6rem" /></button>
        <button @click="removeLink"><Trash size="1.6rem" /></button>
      </div>
    </div>

    <!-- Code -->
    <button
      :class="{ active: props.editor.isActive('code') }"
      @click="toggleActive('code')"
    >
      <CodeXml :size="props.size" />
    </button>

    <!-- Code Block -->
    <button
      :class="{ active: props.editor.isActive('codeBlock') }"
      @click="toggleActive('codeBlock')"
    >
      <SquareCode :size="props.size" />
    </button>

    <!-- Quote -->
    <button
      :class="{ active: props.editor.isActive('blockquote') }"
      @click="toggleActive('blockquote')"
    >
      <Quote :size="props.size" />
    </button>

    <span class="separator"></span>

    <!-- Text Align -->
    <button
      :class="{ active: props.editor.isActive({ textAlign: 'left' }) }"
      @click="toggleActive('textAlign', 'left')"
    >
      <AlignLeft :size="props.size" />
    </button>
    <button
      :class="{ active: props.editor.isActive({ textAlign: 'center' }) }"
      @click="toggleActive('textAlign', 'center')"
    >
      <AlignCenter :size="props.size" />
    </button>
    <button
      :class="{ active: props.editor.isActive({ textAlign: 'right' }) }"
      @click="toggleActive('textAlign', 'right')"
    >
      <AlignRight :size="props.size" />
    </button>
    <button
      :class="{ active: props.editor.isActive({ textAlign: 'justify' }) }"
      @click="toggleActive('textAlign', 'justify')"
    >
      <AlignJustify :size="props.size" />
    </button>

    <span class="space"></span>
  </header>
</template>

<style scoped>
header {
  display: flex;
  align-items: center;
  width: 1200px;
  gap: 0.3rem;
  max-width: 100%;
  padding: 1rem;
  border-bottom: 1px solid #eee;
  overflow-x: auto;
}

button {
  padding: 0.6rem;
  line-height: 0;
  color: #777;
  background-color: #fff;
  border: none;
  border-radius: 1.2rem;
  transition: all 0.3s ease-in-out;
}
button.active {
  color: #007bff;
  background-color: #f3f3f4;
}
button:hover {
  background-color: #f1f1f1;
  cursor: pointer;
}

.container {
  position: relative;
}
.container .popup {
  position: absolute;
  top: 100%;
  left: 50%;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  background-color: #fff;
  border-radius: 1.6rem;
  padding: 0.75rem 0.75rem 0.75rem 1.25rem;
  box-shadow: 0 0 1rem rgba(0, 0, 0, 0.2);
  transform: translate(-50%, 0.5rem);
  transition: all 0.3s ease-in-out;
}
.container .popup input {
  font-size: 1.4rem;
  color: #333;
  placeholder-color: #aaa;
  border: none;
  outline: none;
}
.container .popup button {
  color: #aaa;
  border-radius: 1rem;
}

span.space {
  flex: 1 1 auto;
}
span.separator {
  margin: 0 1rem;
  width: 1px;
  height: 2rem;
  background-color: #eee;
}
</style>

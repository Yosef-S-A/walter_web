<template>
  <div class="col-10 q-pa-md q-mx-md editor_div">
    <div class="editor_header" v-if="editor">
      <q-btn
        @click="editor.chain().focus().toggleBold().run()"
        :class="{ 'is-active': editor.isActive('bold') }"
        icon="mdi-format-bold"
        flat
        round
      ></q-btn>
      <q-btn
        @click="editor.chain().focus().toggleItalic().run()"
        :class="{ 'is-active': editor.isActive('italic') }"
        icon="mdi-format-italic"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleStrike().run()"
        :class="{ 'is-active': editor.isActive('strike') }"
        icon="mdi-format-strikethrough"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleCode().run()"
        :class="{ 'is-active': editor.isActive('code') }"
        icon="mdi-code-tags"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().unsetAllMarks().run()"
        icon="mdi-format-clear"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().setParagraph().run()"
        :class="{ 'is-active': editor.isActive('paragraph') }"
        icon="mdi-format-paragraph"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleHeading({ level: 1 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 1 }) }"
        icon="mdi-format-header-1"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleHeading({ level: 2 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 2 }) }"
        icon="mdi-format-header-2"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleHeading({ level: 3 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 3 }) }"
        icon="mdi-format-header-3"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleHeading({ level: 4 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 4 }) }"
        icon="mdi-format-header-4"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleHeading({ level: 5 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 5 }) }"
        icon="mdi-format-header-5"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleHeading({ level: 6 }).run()"
        :class="{ 'is-active': editor.isActive('heading', { level: 6 }) }"
        icon="mdi-format-header-6"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleBulletList().run()"
        :class="{ 'is-active': editor.isActive('bulletList') }"
        icon="mdi-format-list-bulleted"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleOrderedList().run()"
        :class="{ 'is-active': editor.isActive('orderedList') }"
        icon="mdi-format-list-numbered"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleCodeBlock().run()"
        :class="{ 'is-active': editor.isActive('codeBlock') }"
        icon="mdi-code-braces"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().toggleBlockquote().run()"
        :class="{ 'is-active': editor.isActive('blockquote') }"
        icon="mdi-format-quote-close-outline"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().setHorizontalRule().run()"
        icon="mdi-minus"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().setHardBreak().run()"
        icon="mdi-format-page-break"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().undo().run()"
        icon="mdi-undo"
        flat
        round
      >
      </q-btn>
      <q-btn
        @click="editor.chain().focus().redo().run()"
        icon="mdi-redo"
        flat
        round
      >
      </q-btn>
    </div>

    <div class="editor_content">
      <editor-content :editor="editor" />
    </div>
    <div class="editor_footer"></div>
  </div>
</template>

<script>
import StarterKit from "@tiptap/starter-kit";
// eslint-disable-next-line
import { Editor, EditorContent } from "@tiptap/vue-3";
export default {
  components: {
    EditorContent,
  },
  props: {
    modelValue: {
      type: String,
      default: "",
    },
  },
  emits: ["update:modelValue"],
  data() {
    return {
      editor: null,
    };
  },
  watch: {
    modelValue(value) {
      // HTML
      const isSame = this.editor.getHTML() === value;
      // JSON
      // const isSame = JSON.stringify(this.editor.getJSON()) === JSON.stringify(value)
      if (isSame) {
        return;
      }
      this.editor.commands.setContent(value, false);
    },
  },
  mounted() {
    this.editor = new Editor({
      extensions: [StarterKit],
      content: this.modelValue,
      onUpdate: () => {
        // HTML
        this.$emit("update:modelValue", this.editor.getHTML());
        // JSON
        // this.$emit('update:modelValue', this.editor.getJSON())
      },
    });
  },
  beforeUnmount() {
    this.editor.destroy();
  },
};
</script>

<style>
.editor_header {
  background-color: #c4c4c4;
}
.ProseMirror {
  border: solid 1px #eeeeee;
  border-radius: 0 0 6px 6px;
  height: 350px;
}
</style>

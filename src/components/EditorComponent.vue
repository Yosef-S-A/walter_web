<template>
  <div class="col-10 q-pa-md q-mx-md">
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
      <q-btn
        @click="editor.chain().focus().clearContent().run()"
        icon="mdi-delete-outline"
        flat
        round
        style="color: red"
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
import { Editor, EditorContent, VueNodeViewRenderer } from "@tiptap/vue-3";
import CodeBlockLowlight from "@tiptap/extension-code-block-lowlight";
import { lowlight } from "lowlight";
import CodeBlockComponent from "./CodeBlockComponent.vue";
let EMPTY_OBJECT = {
  type: "doc",
  content: [
    {
      type: "paragraph",
    },
  ],
};
export default {
  components: {
    EditorContent,
  },
  props: {
    modelValue: {
      type: Object,
      default: EMPTY_OBJECT,
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
      // const isSame = this.editor.getHTML() === value;
      // JSON
      const isSame =
        JSON.stringify(this.editor.getJSON()) === JSON.stringify(value);
      if (isSame) {
        return;
      }
      this.editor.commands.setContent(value, false);
    },
  },
  mounted() {
    this.editor = new Editor({
      extensions: [
        StarterKit.configure({
          codeBlock: false,
        }),
        CodeBlockLowlight.extend({
          addNodeView() {
            return VueNodeViewRenderer(CodeBlockComponent);
          },
        }).configure({
          lowlight,
        }),
      ],
      content: this.modelValue,
      onUpdate: () => {
        // HTML
        // this.$emit("update:modelValue", this.editor.getHTML());
        // JSON
        this.$emit("update:modelValue", this.editor.getJSON());
      },
    });
  },
  beforeUnmount() {
    this.editor.destroy();
  },
};
</script>

<style lang="scss">
/* Basic editor styles */
#q-app
  > div
  > div.q-page-container
  > div
  > div:nth-child(n)
  > div
  > div.q-item__section.column.q-item__section--main.justify-center {
  height: 100%;
}
.editor_header {
  background-color: #c4c4c4;
}
.ProseMirror {
  outline: none;
  border: solid 1.5px #b3b3b3;
  min-height: 80px;
  padding: 0.5em;
  font-family: Verdana, Geneva, Tahoma, sans-serif;
  > * + * {
    margin-top: 0.75em;
  }

  ul,
  ol {
    padding: 0 1rem;
  }

  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    line-height: 1.1;
  }

  code {
    background-color: rgba(#616161, 0.1);
    white-space: pre-wrap;
    font-size: 0.8rem;
    padding: 0.5em 0.5em;
    background-color: #e4e4e4;
    color: red;
    font-family: "Courier New", Courier, monospace;
  }

  pre {
    background: #0d0d0d;
    color: #fff;
    font-family: "JetBrainsMono", monospace;
    padding: 0.75rem 1rem;
    border-radius: 0.5rem;

    code {
      color: inherit;
      padding: 0.5em;
      background: none;
      font-size: 0.8rem;
    }
    .hljs-comment,
    .hljs-quote {
      color: #616161;
    }

    .hljs-variable,
    .hljs-template-variable,
    .hljs-attribute,
    .hljs-tag,
    .hljs-name,
    .hljs-regexp,
    .hljs-link,
    .hljs-name,
    .hljs-selector-id,
    .hljs-selector-class {
      color: #f98181;
    }

    .hljs-number,
    .hljs-meta,
    .hljs-built_in,
    .hljs-builtin-name,
    .hljs-literal,
    .hljs-type,
    .hljs-params {
      color: #fbbc88;
    }

    .hljs-string,
    .hljs-symbol,
    .hljs-bullet {
      color: #b9f18d;
    }

    .hljs-title,
    .hljs-section {
      color: #faf594;
    }

    .hljs-keyword,
    .hljs-selector-tag {
      color: #70cff8;
    }

    .hljs-emphasis {
      font-style: italic;
    }

    .hljs-strong {
      font-weight: 700;
    }
  }

  img {
    max-width: 100%;
    height: auto;
  }

  blockquote {
    padding-left: 1rem;
    border-left: 2px solid rgba(#0d0d0d, 0.1);
  }

  hr {
    border: none;
    border-top: 2px solid rgba(#0d0d0d, 0.1);
    margin: 2rem 0;
  }
}
</style>

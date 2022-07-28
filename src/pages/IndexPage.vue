<template>
  <div>
    <editor v-model:modelValue="content" />

    <div class="content">
      <h3 class="text-3xl">Content</h3>
      <pre><code>{{ content }}</code></pre>
    </div>
  </div>
</template>

<script>
import Editor from "../components/EditorComponent.vue";
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
    Editor,
  },
  data() {
    return {
      content: EMPTY_OBJECT,
    };
  },
};
</script>

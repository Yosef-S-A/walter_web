<template>
  <div>
    <editor v-model="content" />

    <div class="content">
      <h3>Content</h3>
      <pre><code>{{ content }}</code></pre>
    </div>
  </div>
</template>

<script>
import Editor from "../components/EditorComponent.vue";

export default {
  components: {
    Editor,
  },
  data() {
    return {
      content: "",
    };
  },
};
</script>

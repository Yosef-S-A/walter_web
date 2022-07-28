<template>
  <div>
    <editor v-model:modelValue="content" />

    <div class="content">
      <h3 class="text-3xl">Content</h3>
      <pre><code>{{ content }}</code></pre>
    </div>

    <q-btn label="Add Option" @click="addOption"></q-btn>

    <q-list v-for="(option, index) in options" :key="index">
      <q-item>
        <q-item-section side>
          <q-checkbox v-model="option.is_correct"></q-checkbox>
        </q-item-section>
        <q-item-section>
          <editor v-model="option.text" />
        </q-item-section>
        <q-item-section>
          <h3 class="text-3xl">Content</h3>
          <pre><code>{{ option.text }}</code></pre>
        </q-item-section>
      </q-item>
      <!-- </div> -->
    </q-list>
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
      options: [],
    };
  },
  methods: {
    addOption() {
      this.options.push({ text: null, is_correct: false });
    },
    async submit_data() {
      // alert("Data submitted")
      const res = await axios.post("http://localhost:3000/question", {
        stem: this.content,
        options: this.options,
      });
      console.log(res.data.data);
    },
  },
};
</script>

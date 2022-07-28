<template>
  <div>
    <editor v-model:modelValue="content" />

    <div class="q-mx-md">
      <q-btn
        label="Add Option"
        @click="addOption"
        class="col-10 q-mx-md"
      ></q-btn>
      <q-btn-dropdown color="primary" label="Tags">
        <q-list>
          <q-item clickable v-close-popup @click="onItemClick">
            <q-item-section>
              <q-item-label>Vue</q-item-label>
            </q-item-section>
          </q-item>

          <q-item clickable v-close-popup @click="onItemClick">
            <q-item-section>
              <q-item-label>Python</q-item-label>
            </q-item-section>
          </q-item>

          <q-item clickable v-close-popup @click="onItemClick">
            <q-item-section>
              <q-item-label>Quasar</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-btn-dropdown>
    </div>

    <q-list v-for="(option, index) in options" :key="index">
      <q-item>
        <q-item-section side>
          <q-checkbox v-model="option.is_correct"></q-checkbox>
        </q-item-section>
        <q-item-section>
          <editor v-model="option.text" />
        </q-item-section>
      </q-item>
    </q-list>
    <div class="q-mx-md">
      <q-btn
        class="col-10 q-mx-md"
        :class="options.length < 2 ? 'disabled' : ''"
        color="deep-orange"
        push
        icon="mdi-content-save"
        label="Save"
        @click="submit_data"
        type="submit"
        :loading="submitting"
      />
    </div>
  </div>
</template>

<script>
import Editor from "../components/EditorComponent.vue";
import axios from "axios";

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

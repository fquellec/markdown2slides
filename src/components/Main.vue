<template>
  <v-container fluid class="fill-height">
    <v-row class="fill-height">
      <v-col cols="6">
        <MarkdownEditor v-model:markdown="markdown"/>
      </v-col>
      <v-col cols="6">
        <MarkdownRenderer v-bind="{markdown}"/>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import MarkdownEditor from '@/components/MarkdownEditor.vue';
import MarkdownRenderer from '@/components/MarkdownRenderer.vue';

export default {
  components: {
    MarkdownEditor,
    MarkdownRenderer
  },
  data() {
    return {
      markdown: '# Your Title'
    }
  },
  watch: {
    markdown(newMarkdown) {
      console.log(newMarkdown)
    }
  }
};
</script>

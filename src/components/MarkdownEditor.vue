<template>
  <v-card flat height="100%">
    <div class="d-flex">
      <v-btn-toggle
        v-model="toggleStyle"
        background-color="primary"
        multiple
      >
        <v-btn value="bold" @click="setBold">
          <v-icon>mdi-format-bold</v-icon>
        </v-btn>
        <v-btn value="italic">
          <v-icon>mdi-format-italic</v-icon>
        </v-btn>
        <v-btn value="underline">
          <v-icon>mdi-format-underline</v-icon>
        </v-btn>
        <v-btn value="header-1">
          <v-icon>mdi-format-header-1</v-icon>
        </v-btn>
        <v-btn value="header-2">
          <v-icon>mdi-format-header-2</v-icon>
        </v-btn>
        <v-btn value="header-3">
          <v-icon>mdi-format-header-3</v-icon>
        </v-btn>
        <v-divider vertical inset></v-divider>
        <v-btn value="link">
          <v-icon>mdi-link-plus</v-icon>
        </v-btn>
        <v-btn value="image">
          <v-icon>mdi-image-plus</v-icon>
        </v-btn>
        <v-btn value="theme">
          <v-icon>mdi-format-title</v-icon>
        </v-btn>
      </v-btn-toggle>      
    </div>
    <textarea 
      type="text"
      ref="textarea"
      :value="markdown" 
      @input="$emit('update:markdown', $event.target.value)"
    ></textarea>
  </v-card>
</template>

<script>
import { toHandlerKey } from 'vue';

export default {
  props: {
    markdown: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      toggleStyle: null
    }
  },
  watch: {
    toggleStyle(newValue){
      console.log(newValue)
    }
  },
  emits: ['update:markdown'],
  methods: {
    setCursorPosition(position) {
      this.$refs.textarea.selectionStart = position;
      this.$refs.textarea.selectionEnd = position;
      this.$refs.textarea.focus();
    },
    getCursorPosition() {
      return this.$refs.textarea.selectionStart;
    },
    setBold() {
      const startPosition = this.$refs.textarea.selectionStart;
      const endPosition = this.$refs.textarea.selectionEnd;
      console.log(startPosition, endPosition)
      const newText = `${this.markdown.slice(0, startPosition)}**${this.markdown.slice(startPosition, endPosition)}**${this.markdown.slice(endPosition)}`;
      this.$emit('update:markdown', newText)
      this.$refs.textarea.selectionStart = startPosition + 2;
      this.$refs.textarea.selectionEnd = endPosition + 4;
      this.$refs.textarea.focus();
    },
    setItalic() {
    
    },
    setUnderline() {
    
    },
    setHeader(size) {
    
    },
    addLink() {
    
    },
    addImage() {
    
    }
  }
};
</script>

<style scoped>
textarea {
  border: none;
  resize: none;
  outline: none;
  background-color: #f6f6f6;
  font-size: 14px;
  font-family: "Monaco", courier, monospace;
  padding: 20px;
  width: 100%;
  height: 100%;
}
</style>

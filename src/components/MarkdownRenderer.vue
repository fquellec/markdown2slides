<template>
  <v-card flat height="100%">
    <div class="d-flex">
      <v-spacer></v-spacer>
      <v-btn-toggle
        v-model="toggleStyle"
        background-color="primary"
        multiple
      >
        <v-btn value="fullscreen" @click="fullscreen">
          <v-icon>mdi-fullscreen</v-icon>
        </v-btn>
        <v-btn value="download" @click="download">
          <v-icon>mdi-download</v-icon>
        </v-btn>
      </v-btn-toggle>
      
    </div>
    <div id="content" class="content" v-html="html"></div>
  </v-card>
</template>

<script>
import Marpit from '@marp-team/marpit'
import theme from "@/assets/themes/default.css?inline"
import bespoke from '@/assets/bespoke/bespoke'

export default {
  props: {
    markdown: {
      type: String,
      default: ''
    }
  },
  data() {
    //const marpit = new Marpit()
    //marpit.themeSet.set('default', theme)

    return {
      // marpit,
      html: '',
      css: ''
    };
  },
  watch: {
    markdown(newMarkdown) {
      this.renderMarkdown(newMarkdown);
    }
  },
  mounted(){
    this.renderMarkdown(this.markdown)
  },
  methods: {
    renderMarkdown(markdown) {
      const marpit = new Marpit({
        inlineSVG: true,
        html: true
      })
      marpit.themeSet.default = marpit.themeSet.add(theme)
      const { html, css } = marpit.render(markdown)
      this.html = `<style>${css}</style>${html}`
    },
    download() {
      const htmlBespoke = `
      <!DOCTYPE html>
        <html lang="C">
          <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width,height=device-height,initial-scale=1.0">
            <meta name="apple-mobile-web-app-capable" content="yes">
            <meta http-equiv="X-UA-Compatible" content="ie=edge">
            <meta property="og:type" content="website"><meta name="twitter:card" content="summary">
            <style>${bespoke.css}</style>
          </head>
          <body>
          ${bespoke.html}
          ${this.html.replace('class="marpit"', 'id=":$p" class="marpit"')}
          ${bespoke.js}
          </body>
        </html>
      `
      const blob = new Blob([htmlBespoke], { type: "text/html;charset=utf-8" });
      const url = URL.createObjectURL(blob);
      const link = document.createElement("a");
      link.href = url;
      link.download = "presentation.html"; 
      document.body.appendChild(link);
      link.click();

      // Clean up the object URL
      URL.revokeObjectURL(url);

      // Remove the temporary link element
      document.body.removeChild(link);
    },
    fullscreen() {
      console.log('fullscreen')
    }
  }
};
</script>

<style scoped>
.marpit {
  width: 100%; 
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: auto;
}

.content {
  width: 100%; 
  height: 100%;
  background-color: #FAFAFA;
}

</style>

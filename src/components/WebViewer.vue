<template>
  <div id="webviewer" ref="viewer"></div>
</template>

<script>
import WebViewer from "@pdftron/webviewer";
export default {
  name: "WebViewer",
  props: {
    url: String,
  },
  //Initialize the webviewer instance in the mounted lifecycle hook

  mounted() {
    WebViewer(
      {
        path: `${process.env.BASE_URL}webviewer`,
        initialDoc: this.url,
      },
      this.$refs.viewer
    ).then((instance) => {
        instance.setTheme("dark");
    });
  },
};
</script>

<style scoped>
div {
  width: 100%;
  height: 100vh;
}
</style>
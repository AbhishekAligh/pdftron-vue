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
  data() {
    return {
      annotations: [
        { page: 2, x: 150, y: 150, w: 50, h: 20, id: "1" },
        { page: 1, x: 80, y: 90, w: 50, h: 20, id: "2" },
        { page: 1, x: 120, y: 120, w: 50, h: 20, id: "3" },
      ],
    };
  },
  mounted() {
    WebViewer(
      {
        path: `${process.env.BASE_URL}webviewer`,
        initialDoc: this.url,
      },
      this.$refs.viewer
    ).then((instance) => {
      const { docViewer, annotManager, Annotations } = instance;
      instance.setTheme("dark");
      instance.disableElements(["searchButton"]);
      docViewer.setMargin(40);

      docViewer.on("documentLoaded", () => {

        for (let annotId = 0; this.annotations.length; annotId++) {
          let annot = this.annotations[annotId];
          annotManager.setCurrentUser("Abhishek Aligh");
          const rectangleAnnot = new Annotations.RectangleAnnotation();
          rectangleAnnot.PageNumber = annot.page;
          rectangleAnnot.X = annot.x;
          rectangleAnnot.Y = annot.y;
          rectangleAnnot.Width = annot.w;
          rectangleAnnot.Height = annot.h;
          rectangleAnnot.Author = annotManager.getCurrentUser();
          rectangleAnnot.FillColor = new Annotations.Color(100, 1500, 130, 0.1);
          rectangleAnnot.StrokeColor = new Annotations.Color(0, 0,0);
          rectangleAnnot.StrokeThickness = 5;
          rectangleAnnot.setCustomData("id", annot.id);

          annotManager.addAnnotation(rectangleAnnot);
          annotManager.redrawAnnotation(rectangleAnnot);
        }
      });
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
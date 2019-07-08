<template>
  <div>
    <input type="file" id="selectPDF" v-on:change="LoadPDF" accept="application/pdf" />
    <div id="my_pdf_viewer">
      <div id="canvas_container">
        <canvas id="pdf_renderer"></canvas>
      </div>
      <div id="navigation_controls">
        <button id="go_previous"  @click="Previous">Previous</button>
        <input id="current_page" value="1" type="number" />
        <button id="go_next"  @click="Next">Next</button>
      </div>
      <div id="zoom_controls">  
        <button id="zoom_in"  @click="ZoomIn">+</button>
        <button id="zoom_out"  @click="ZoomOut">-</button>
      </div>
    </div>
    <vue-pdfjs url="New_Horizons.pdf" :type="1"></vue-pdfjs>
  </div>
</template>

<script>
import axios from 'axios';
import { PDFJSLIB, myState } from 'pdfjs';
import pdf from 'vue-pdf';
import vuePdfjs from 'vue-pdfjs'

export default {
  components: {
    vuePdfjs
  },
  data() {
    return {
      pdfjsLib: '',
      result: '',
    };
  },
  // mounted() {
  //   this.LoadPDF(e);
  // },
  // created() {
  //   axios
  //     .get(`http://localhost:54490/APITest/Index`)
  //     .then(response => {
  //       this.result = response.data.message
  //     })
  //     .catch(error => {
  //       this.result = "check console";
  //       console.log(error);
  //     });
  // }
  methods: {
    LoadPDF(e) {
      console.log('e: ', e);
      console.log(PDFJSLIB);
      PDFJSLIB.getDocument('../pdfFiles/New_Horizons.pdf').then((pdf) => {
      console.log('pdf: ', pdf);
      myState.pdf = pdf;
        this.render();
      });
    },
    render() {
      var canvas = document.getElementById("pdf_renderer");
      var ctx = canvas.getContext('2d');
      myState.pdf.getPage(myState.currentPage).then((page) => {
        var viewport = page.getViewport(myState.zoom);
        canvas.width = viewport.width;
        canvas.height = viewport.height;
        page.render({
          canvasContext: ctx,
          viewport: viewport
          });
      });
    },
    Previous(e) {
      document.getElementById('go_previous').addEventListener('click', (e) => {
          if(myState.pdf == null || myState.currentPage == 1) return;
          myState.currentPage -= 1;
          document.getElementById('current_page').value = myState.currentPage;
          this.render();
      });
    },
    Next(e) {
      document.getElementById('go_next').addEventListener('click', (e) => {
          if(myState.pdf == null || myState.currentPage > myState.pdf._pdfInfo.numPages)
            return;
      
          myState.currentPage += 1;
          document.getElementById('current_page').value = myState.currentPage;
          this.render();
      });
    },
    ZoomIn(e){
      document.getElementById('zoom_in').addEventListener('click', (e) => {
          if(myState.pdf == null) return;
          myState.zoom += 0.5;
          this.render();
      });
    },
    ZoomOut(e){
      document.getElementById('zoom_out').addEventListener('click', (e) => {
          if(myState.pdf == null) return;
          myState.zoom -= 0.5;
          this.render();
      });
    }
  }
};
</script>
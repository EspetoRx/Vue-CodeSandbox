<style scoped>
.modal {
  display: block;
  overflow: auto !important;
  z-index: 1059;
}
.babado {
  background-color: rgba(0, 0, 0, 0.2);
}
.modal-dialog {
  z-index: 1;
}
footer {
  padding-bottom: 0px;
}
.modal {
  padding: 0 !important;
}
.modal .modal-dialog {
  width: 100%;
  max-width: none;
  height: 100%;
  margin: 0;
}
.modal .modal-content {
  height: 100%;
  border: 0;
  border-radius: 0;
}
.modal .modal-body {
  overflow-y: auto;
}
::-webkit-scrollbar {
  width: 0px;
}
#vuePdfApp {
  height: 82vh !important;
}
</style>
<template>
  <div>
    <form>
      <transition-group
        name="custom-classes-transition"
        enter-active-class="animated slideInDown"
        leave-active-class="animated slideOutUp"
        v-on:after-leave="afterLeave"
      >
        <div
          class="modal-backdrop text-dark babado"
          v-if="this.modalOpen"
          :key="8"
        ></div>
        <div
          id="modal_pdf_reader"
          class="modal"
          role="dialog"
          v-if="this.modalOpen"
          :key="9"
        >
          <div id="modal_body" class="modal-dialog" role="document">
            <div class="modal-content">
              <div class="modal-header">
                <h5 class="modal-title">View file</h5>
                <button
                  type="button"
                  class="btn bg-danger btn-danger"
                  @click.prevent="close"
                  aria-label="Fechar"
                >
                  <span aria-hidden="true"><i class="fa fa-times"></i></span>
                </button>
              </div>
              <div class="modal-body p-0 overflow-hidden">
                <main>
                  <!-- <object :data="this.urlSaved" type="application/pdf" width="100%" style="height: 82.5vh">
                                    <p>Alternative text - include a link <a href="myfile.pdf">to the PDF!</a></p>
                                </object> -->
                  <vue-pdf-app
                    :pdf="this.urlDownload"
                    v-if="this.enablePdfReader"
                    theme="light"
                  ></vue-pdf-app>
                </main>
              </div>
              <div class="modal-footer flex-row justify-content-between">
                <div>
                  <button
                    type="button"
                    class="btn btn-danger"
                    @click.prevent="close"
                  >
                    <i class="fa fa-times"></i> Close
                  </button>
                </div>
                <div>
                  <slot name="buttons"></slot>
                </div>
                <div>
                  <button
                    v-if="urlDownload != ''"
                    class="btn btn-outline-dark"
                    @click.prevent="ImprimirDuplicata"
                  >
                    <i class="fa fa-print"></i> Print
                  </button>
                  <a
                    v-if="urlDownload != ''"
                    :href="urlSaved"
                    target="_blank"
                    class="btn btn-outline-info"
                    ><i class="fa fa-download"></i> Download</a
                  >
                </div>
              </div>
            </div>
          </div>
        </div>
      </transition-group>
    </form>
  </div>
</template>
<script>
import VuePdfApp from "vue-pdf-app";
// import this to use default icons for buttons
import "vue-pdf-app/dist/icons/main.css";
import printJS from "print-js";
export default {
  name: "modal",
  data() {
    return {
      wLocationHost: "https://" + window.location.host + "/",
      pdfApp: {},
      urlSaved: null,
      enablePdfReader: false,
    };
  },
  props: {
    modalOpen: {
      required: true,
    },
    urlDownload: {
      default: null,
      required: true,
    },
  },
  mounted() {
    document.addEventListener("keydown", (e) => {
      if (this.modalOpen) {
        if (e.keyCode === 27) {
          this.close(e);
        }
      }
    });
  },
  components: {
    VuePdfApp,
  },
  methods: {
    close(e) {
      this.$emit("close");
    },
    ImprimirDuplicata() {
      printJS({
        printable: this.urlDownload,
        type: "pdf",
        showModalFile: true,
      });
    },
    afterLeave() {
      this.urlSaved = null;
      this.enablePdfReader = false;
    },
  },
  watch: {
    urlDownload: function () {
      console.log(this.urlDownload);
      if (this.urlDownload !== this.urlSaved && this.urlDownload !== "") {
        this.urlSaved = this.urlDownload;
      }
    },
    modalOpen: async function () {
      await this.$nextTick();
      if (this.modalOpen === true) {
        this.enablePdfReader = true;
      }
    },
  },
};
</script>
<template>
  <div>
    <table class="table">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Info</th>
          <th scope="col">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th scope="row">1</th>
          <td>Mark</td>
          <td>
            <button
              type="button"
              class="btn btn-sm btn-info"
              @click.prevent="
                openModal(
                  'https://file-examples-com.github.io/uploads/2017/10/file-sample_150kB.pdf'
                )
              "
            >
              Check PDF
            </button>
          </td>
        </tr>
        <tr>
          <th scope="row">2</th>
          <td>Jacob</td>
          <td>
            <button
              type="button"
              class="btn btn-sm btn-info"
              @click.prevent="
                openModal(
                  'https://file-examples-com.github.io/uploads/2017/10/file-example_PDF_500_kB.pdf'
                )
              "
            >
              Check PDF
            </button>
          </td>
        </tr>
        <tr>
          <th scope="row">3</th>
          <td>Larry</td>
          <td>
            <button
              type="button"
              class="btn btn-sm btn-info"
              @click.prevent="
                openModal(
                  'https://file-examples-com.github.io/uploads/2017/10/file-example_PDF_1MB.pdf'
                )
              "
            >
              Check PDF
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <modal-pdf-reader
      :modalOpen="this.openModalReader"
      :urlDownload="this.pdfLink"
      @close="closeModal()"
    ></modal-pdf-reader>
  </div>
</template>

<script>
import ModalPDFReader from "./ModalPDFReader.vue";
export default {
  name: "HelloWorld",
  data() {
    return {
      openModalReader: false,
      pdfLink: "",
    };
  },
  methods: {
    openModal(pdfLink) {
      this.pdfLink = pdfLink;
      this.openModalReader = !this.openModalReader;
    },
    closeModal() {
      this.openModalReader = !this.openModalReader;
      this.pdfLink = "";
    },
  },
  components: {
    "modal-pdf-reader": ModalPDFReader,
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

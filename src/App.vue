<template>
  <div id="app">
    <table id="my-table" style="margin-bottom: 30px;">
      <thead>
        <th>id</th>
        <th>text</th>
        <th>today</th>
      </thead>
      <tbody>
        <tr v-for="entry in table" :key="entry.id">
          <td>{{ entry.id }}</td>
          <td>{{ entry.text }}</td>
          <td>{{ entry.today }}</td>
        </tr>
      </tbody>
    </table>
    <!-- buttons -->
    <button @click="pdfFromHTML">generate PDF from html</button>
    <button @click="pdfFromData">generate PDF from data</button>
  </div>
</template>

<script>
// import HelloWorld from "./components/HelloWorld";
import jsPDF from "jspdf";
import "jspdf-autotable";
export default {
  name: "App",
  components: {
    // HelloWorld
  },
  data: function() {
    return {
      table: [
        {
          id: 1,
          text: "test 1",
          today: new Date()
        },
        {
          id: 3,
          text: "test 3",
          today: new Date()
        },
        {
          id: 2,
          text: "test 2",
          today: new Date()
        }
      ]
    };
  },
  methods: {
    // generating from data
    pdfFromData: function() {
      var columns = ["ID", "Text", "Today"];
      var rows = this.table.map(function(data) {
        return [data.id, data.text, data.today];
      });

      var doc = new jsPDF();
      // creating text
      doc.text("Test Data", doc.internal.pageSize.getWidth() / 2, 10, {
        align: "center"
      });
      // Generating table
      doc.autoTable(columns, rows);
      doc.save("table.pdf");
    },
    // generating from table tag @Html
    pdfFromHTML: function() {
      var columns = ["ID", "Text", "Today"];
      const doc = new jsPDF();
      // creating text
      doc.text("Test Data", doc.internal.pageSize.getWidth() / 2, 40, {
        align: "center"
      });
      // Generating table
      doc.autoTable({
        html: "#my-table",
        startY: 50,
        columnStyles: { 0: { halign: "center", fillColor: [0, 255, 0] } },
        columns
      });
      doc.save("table.pdf");
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

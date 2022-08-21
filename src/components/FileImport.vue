<template>
  <div>
    <div class="container">
      <div class="panel panel-sm">
        <div class="panel-heading">
          <h3>Choose preferred file to import</h3>
        </div>
        <div class="panel-body">
          <div class="form-group">
            <label for="csv_file" class="control-label col-sm-3 text-right"
              >CSV file to import</label
            >
            <div class="col-sm-9">
              <label for="file-upload" class="custom-file-upload">
                <i class="fa fa-cloud-upload"></i> Custom Upload
              </label>
              <input
                type="file"
                id="csv_file"
                name="csv_file"
                class="form-control custom-file-upload"
                @change="loadData($event)"
              />
            </div>
          </div>
          <div class="col-sm-offset-3 col-sm-9">
            <button class="button btn-primary" @click="enableCsvData">
              Parse CSV
            </button>
          </div>
        </div>
      </div>
      <div class="d-flex justify-content-center">
        <table v-if="showCsvData">
          <thead>
            <tr>
              <th
                v-for="key in headerList"
                :key="key"
                :class="{ active: true }"
              >
                {{ key }}
              </th>
            </tr>
          </thead>
          <tr v-for="item in csvData" :key="item">
            <td v-for="key in headerList" :key="key">
              {{ csv[key] }}
            </td>
          </tr>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import "../stylesheet/fileImport.css";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      headerList: [],
      csvData: [],
      showCsvData: false,
    };
  },
  methods: {
    enableCsvData() {
      this.showCsvData = true;
    },
    capitalize: function (str) {
      return str.charAt(0).toUpperCase() + str.slice(1);
    },
    getData(csv) {
      let result = [];
      let lines = csv.split("\n");
      let headers = lines[0].split(",");
      lines.map(function (line, indexLine) {
        if (indexLine < 1) return;
        let obj = {};
        let currentline = line.split(",");
        headers.map((header, index) => {
          obj[header] = currentline[index];
        });
        result.push(obj);
      });
      result.pop();
      return result;
    },
    loadData(e) {
      if (window.FileReader) {
        let reader = new FileReader();
        reader.readAsText(e.target.files[0]);
        reader.onload = (event) => {
          var csv = event.target.result;
          this.csvData = this.getData(csv);
        };
        reader.onerror = function (evt) {
          if (evt.target.error.name == "NotReadableError") {
            alert("File is not accesible");
          }
        };
      } else {
        alert("File import is not supported in this browser.");
      }
    },
  },
};
</script>

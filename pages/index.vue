<template>
  <v-container data-app>
    <v-btn to="/scheme" left depressed>Create Scheme</v-btn>
    <v-file-input
      v-model="csv"
      class="d-none"
      id="fileUpload"
      single-line
      label="Upload CSV data file"
      accept=".csv, application/vnd.ms-excel"
      @change="processSelectedFiles"
    ></v-file-input>

    <v-row v-if="csv">
      <v-col cols="3">
        <v-select
          single-line
          :items="employee"
          v-model="employeeData"
          label="Employee"
          outlined
        ></v-select>
      </v-col>
      <v-col cols="3">
        <v-select
          v-model="monthData"
          single-line
          :items="month"
          label="Month"
          outlined
        ></v-select>
      </v-col>
      <v-col cols="3">
        <v-select
          v-model="yearData"
          single-line
          :items="year"
          label="Year"
          outlined
        ></v-select>
      </v-col>
      <v-col cols="3">
        <v-select
          v-model="productData"
          single-line
          :items="product"
          label="Product"
          outlined
        ></v-select>
      </v-col>
      <v-col v-if="csv" cols="12">
        <v-btn @click="calculateScheme" class="mt-2" outlined color="indigo">
          Calculate Benefit</v-btn
        >
      </v-col>
    </v-row>
    <div v-if="results.length">
      <v-data-table
        :headers="tableHeaders"
        :items-per-page="5"
        :items="results"
      ></v-data-table>
    </div>
  </v-container>
</template>

<script>
import { parse } from "papaparse";
export default {
  data() {
    return {
      csv: null,
      filteredData: {},
      tableHeaders: [],
      employeeData: "",
      monthData: "",
      yearData: "",
      employee: [],
      month: [],
      year: [],
      product: [],
      productData: "",
      empData: {},
      results: [],
      schemes: [
        {
          scheme_id: 1, //Mongo responsi
          scheme_name: "scheme1",
          condition_group: [
            {
              condition_group_name: "group1",
              benefits: [
                {
                  type: "fixed",
                  amount: 5000,
                },
                {
                  type: "variable",
                  kpi_condition_name: "k0",
                  percentage: 2,
                },
              ],
              conditions: [
                {
                  kpi_condition_name: "k0",
                  kpi_min_value: 60,
                  kpi_max_value: 61,
                },
                {
                  kpi_condition_name: "k1",
                  kpi_min_value: 70,
                  kpi_max_value: 80,
                },
              ],
            },
            {
              condition_group_name: "group2",
              conditions: [
                {
                  kpi_condition_name: "k2",
                  kpi_min_value: 40,
                  kpi_max_value: 50,
                },
                {
                  kpi_condition_name: "k1",
                  kpi_min_value: 70,
                  kpi_max_value: 80,
                },
                {
                  kpi_condition_name: "k3",
                  kpi_min_value: 1,
                  kpi_max_value: Infinity,
                },
              ],
              benefits: [
                {
                  type: "fixed",
                  amount: 5000,
                },
                {
                  type: "variable",
                  kpi_condition_name: "k3",
                  percentage: 100,
                },
              ],
            },
          ],
        },
      ],
    };
  },
  methods: {
    calculateBenefits() {
      let amount = 0;
      this.schemes[0].condition_group.forEach((group) => {
        // console.log(group);
        const success = group.conditions.every((cond) => {
          // console.log(cond.kpi_min_value);
          // console.log(cond.kpi_max_value);
          // console.log(
          //   +this.filteredData[cond.kpi_condition_name.toUpperCase()].kpi_value
          // );
          let kpi_value =
            +this.filteredData[cond.kpi_condition_name.toUpperCase()].kpi_value;
          if (cond.kpi_min_value === Infinity) {
            return kpi_value < cond.kpi_max_value;
          } else if (cond.kpi_max_value === Infinity) {
            return kpi_value > cond.kpi_min_value;
          } else {
            return (
              cond.kpi_min_value < kpi_value && kpi_value < cond.kpi_max_value
            );
          }
        });

        if (success) {
          group.benefits.forEach((ben) => {
            if (ben.type === "fixed") {
              amount += ben.amount;
            } else {
              amount +=
                +this.filteredData[ben.kpi_condition_name.toUpperCase()]
                  .kpi_val_in_rupees *
                (ben.percentage / 100);
            }
          });
        }
        console.log(amount);
      });
      console.log(amount);
    },
    calculateScheme() {
      this.empData = this.results.forEach((res) => {
        if (
          res.emp_id === this.employeeData &&
          res.month === this.monthData &&
          res.year === this.yearData &&
          res.product === this.productData
        ) {
          this.filteredData[res.kpi_id] = res;
        }
      });

      this.calculateBenefits();
    },
    processUniqueVal(arr, val) {
      if (arr.includes(val)) {
        return false;
      }
      arr.push(val);
    },
    async processSelectedFiles() {
      console.log(`[this.csv]: `, this.csv);
      if (this.csv) {
        if (
          this.csv.type === "application/vnd.ms-excel" ||
          this.csv.name.endsWith(".csv")
        ) {
          parse(this.csv, {
            header: true,
            complete: async (results, file) => {
              console.log("Parsing complete:", results);
              if (results.data && results.data.length) {
                results.data.forEach((data) => {
                  this.processUniqueVal(this.employee, data.emp_id);
                  this.processUniqueVal(this.month, data.month);
                  this.processUniqueVal(this.year, data.year);
                  this.processUniqueVal(this.product, data.product);
                });
                results.meta.fields.forEach((f) => {
                  this.tableHeaders.push({
                    text: f,
                    value: f,
                  });
                });
                this.tableHeaders[0] = {
                  ...this.tableHeaders[0],
                  align: "start",
                  sortable: false,
                };
                // console.log(this.tableHeaders);
                this.results.push(...results.data);
              }
            },
          });
        } else {
          alert("Only CSV file are accepted as of now.");
        }
      }
    },
  },
};
</script>

<style>
table {
  text-align: left !important;
  margin-top: 2rem;
}
</style>

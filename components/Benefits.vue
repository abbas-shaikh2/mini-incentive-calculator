<template>
  <v-row>
    <v-col cols="4">
      <v-select
        v-model="ben_type"
        :items="ben_type_list"
        label="Type of Benefit"
      ></v-select>
    </v-col>
    <v-col cols="8">
      <div v-if="ben_type">
        <div v-if="ben_type === 'Fixed'">
          <v-text-field
            v-model="fix_val"
            label="Enter Fixed Value"
            type="Number"
            required
          ></v-text-field>
        </div>
        <div class="captain_america" v-else>
          <v-text-field
            v-model="per_val"
            label="Enter Percent"
            type="Number"
            required
          ></v-text-field>

          <v-select
            v-model="kpi_name"
            :items="kpi_name_list"
            label="KPI name"
          ></v-select>
        </div>
      </div>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      ben_type: "",
      ben_type_list: ["Fixed", "Variable"],
      fix_val: "",
      per_val: "",
      kpi_name_list: ["k0", "k1", "k2", "k3", "k4"],
      kpi_name: "",
    };
  },
};
</script>

<style>
.captain_america {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 1rem;
}
</style>

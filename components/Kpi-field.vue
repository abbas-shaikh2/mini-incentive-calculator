<template>
  <v-row>
    <v-col cols="3">
      <v-select
        v-model="kpi_name"
        :items="kpi_name_list"
        label="KPI name"
      ></v-select>
    </v-col>
    <v-col v-if="kpi_name" cols="3">
      <v-select
        v-model="cond_val"
        :items="cond_list"
        label="Should be"
      ></v-select>
    </v-col>
    <v-col v-if="cond_val" cols="5">
      <div v-if="cond_val === 'greater than' || cond_val === 'less than'">
        <v-text-field
          v-model="kpi_val"
          label="KPI Value"
          type="Number"
          required
        ></v-text-field>
      </div>
      <div class="iron-man" v-else>
        <v-text-field
          v-model="kpi_min_range_val"
          label="Min val"
          type="Number"
          required
        ></v-text-field>
        <v-text-field
          v-model="kpi_max_range_val"
          label="Max Val"
          type="Number"
          required
        ></v-text-field>
      </div>
    </v-col>
  </v-row>
</template>

<script>
export default {
  data() {
    return {
      kpi_name_list: ["k0", "k1", "k2", "k3", "k4"],
      cond_list: ["greater than", "less than", "in range of"],
      kpi_name: "",
      cond_val: "",
      kpi_val: "",
      kpi_min_range_val: "",
      kpi_max_range_val: "",
    };
  },
};
</script>

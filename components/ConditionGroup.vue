<template>
  <v-row style="margin: 1rem 0">
    <v-col cols="12">
      <v-text-field
        v-model="conditionGroupName"
        label="Condition Group Name"
        required
      ></v-text-field>
    </v-col>

    <v-col cols="6">
      <h1>KPI</h1>
      <div v-for="n in kpi_cond" :key="n">
        <KPI />
      </div>
      <add-button
        @clicked="kpi_cond = kpi_cond + 1"
        message="Add new KPI group"
      />
    </v-col>
    <v-col cols="6">
      <h1>Benefits</h1>
      <div v-for="n in benefits" :key="n">
        <Benefits />
      </div>
      <add-button @clicked="benefits = benefits + 1" message="Add Benefit" />
    </v-col>
  </v-row>
</template>

<script>
import AddButton from "./AddButton.vue";
import Benefits from "./Benefits.vue";
import KPI from "./Kpi.vue";

export default {
  components: { Benefits, KPI, AddButton },
  data() {
    return {
      conditionGroupName: "",
      kpi_cond: 1,
      benefits: 1,
    };
  },
};
</script>

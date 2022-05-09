<template>
  <div data-app>
    <v-text-field
      v-model="groupName"
      label="Enter Group Name"
      required
    ></v-text-field>
    <div v-for="n in kpi_cond_count" :key="n">
      <kpi-field />
    </div>
    <add-button
      @clicked="kpi_cond_count = kpi_cond_count + 1"
      message="Add Condition"
    />
  </div>
</template>

<script>
import AddButton from "./AddButton.vue";
import KpiField from "./Kpi-field.vue";
export default {
  components: { KpiField, AddButton },
  data() {
    return {
      groupName: "",
      kpi_cond_count: 1,
    };
  },
};
</script>

<style>
.iron-man {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 3px;
}
</style>

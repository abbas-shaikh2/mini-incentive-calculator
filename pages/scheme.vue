<template>
  <v-container>
    <v-row>
      <v-col cols="12">
        <v-text-field
          v-model="schemeName"
          label="Enter Scheme Name"
          required
        ></v-text-field>
        <div v-for="n in conditionGroup" :key="n">
          <condition-group />
        </div>
        <add-button
          @clicked="conditionGroup = conditionGroup + 1"
          message="Add new Conidition Group"
        />
      </v-col>
    </v-row>
    <v-btn dark>Save</v-btn>
  </v-container>
</template>

<script>
import AddButton from "~/components/AddButton.vue";
import ConditionGroup from "~/components/ConditionGroup.vue";

export default {
  components: { ConditionGroup },
  data() {
    return {
      schemeName: "",
      addHovered: false,
      conditionGroup: 1,
    };
  },
  components: {
    AddButton,
  },
};
</script>

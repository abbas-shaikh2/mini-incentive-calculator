<template>
  <div
    class="add"
    :style="`opacity: ${addHovered ? '90%' : '0%'}`"
    @mouseover="addHovered = true"
    @mouseleave="addHovered = false"
  >
    <v-tooltip top>
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          @click="handleClick"
          v-bind="attrs"
          v-on="on"
          small
          fab
          color="indigo"
        >
          <v-icon dark> mdi-plus </v-icon>
        </v-btn>
      </template>
      <span>{{ message }}</span>
    </v-tooltip>
  </div>
</template>

<script>
export default {
  data() {
    return {
      addHovered: false,
    };
  },
  props: ["message"],
  methods: {
    handleClick() {
      this.$emit("clicked");
    },
  },
};
</script>

<style>
.add {
  text-align: center;
  position: relative;
  border-top: 1px dashed #111;
  margin: 2rem 0;
  height: 10px;
}

.add .v-btn {
  position: absolute;
  top: -20px;
}
</style>

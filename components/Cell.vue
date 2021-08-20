<template>
  <text class="cell">
    <text v-if="value !== ''">{{ value }}</text>
  </text>
</template>

<script>
export default {
  props: {
    value: String,
  },
};
</script>
<style>
.cell {
  width: 64px;
  height: 64px;
  border-color: rgb(75, 75, 75);
  border-radius: 2px;
  border-style: solid;
  border-width: 2px;
  font-size: 42px;
  flex-direction: column;
  text-align: center;
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
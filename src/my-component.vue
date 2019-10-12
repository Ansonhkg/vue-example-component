<template>
  <div class="container">This is an example component.</div>
</template>

<script>
export default {
  mounted() {
    console.info("Example component is mounted!");
  }
};
</script>

<style>
.container {
  background-color: red;
  padding: 10px;
  width: 100px;
  height: 100px;
  position: absolute;
}
</style>
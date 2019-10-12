<template>
  <div class="example-component--container">
    <span>This is an example component.</span>
  </div>
</template>

<script>
export default {
  name: "example-component",
  mounted() {
    console.warn("Example Component is mounted.");
  }
};
</script>

<style>
.example-component--container {
  background-color: red;
  width: 200px;
  height: 200px;
  display: flex;
  justify-content: center;
  padding: 10px;
  border: 10px solid black;
  box-sizing: border-box;
  text-align: center;
  position: fixed;
}
.example-component--container > span {
  margin: auto;
}
</style>
<script>
export default {
  name: "FooterItem",
};
</script>

<template>
  <footer class="text-white p-1">
    <div class="d-flex justify-content-around">
      <p>&longleftarrow; By Nova Siska Nurfadilah </p>
      <p></p>
      <p>&copy; My Al-Qur'an </p>
    </div>
  </footer>
</template>

<style scoped>
footer {
  font-family: Joyce;
  background-color: #18497e;
  position: fixed;
  bottom: 0;
  width: 100%;
}
@keyframes hue-rotate {
  to {
    filter: hue-rotate(90deg);
  }
}
</style>

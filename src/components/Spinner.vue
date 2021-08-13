<template>
  <div class="spin"></div>
</template>

<script>
export default {
  name: "Spinner"
}
</script>

<style scoped>
  .spin{
    display: block;
    width: 20px;
    height: 20px;
    margin: 20px auto;
    border: 3px solid transparent;
    border-radius: 50%;
    border-top-color: #E8570BED;
    animation: spin 1s ease infinite;
  }
  @keyframes spin {
    to {transform: rotateZ(360deg)}
    
  }
</style>
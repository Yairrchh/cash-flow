<script>
export default {
  name: 'SplashScreen'
}
</script>

<template>
  <div class="splashScreen">
    <img src="../assets/cash-flow.svg" alt="logo" />
    <h1><span>Cash</span>Flow</h1>
  </div>
</template>

<style>
.splashScreen {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  height: 100vh;
}
h1 {
  color: var(--brand-blue);
}
h1 span {
  color: var(--brand-green);
}
</style>

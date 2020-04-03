<template>
    <div class="heroImage" />
</template>
<script>
export default {
  name: 'HeroImage',
};
</script>

<style lang="scss" scoped>
    .heroImage{
        width: 100%;
        height: 100vh;
        position: fixed;
        z-index: -1;
        top: 0;
        background: url('../assets/bg.jpg') no-repeat;
        background-size:cover;
        background-position: 100% 0%;
        transform: translateY(0);
    }
</style>

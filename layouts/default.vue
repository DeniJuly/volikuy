<template>
  <div>
    <Nuxt />
  </div>
</template>

<style>
  @font-face {
    font-family: 'poppins-light';
    src: url('../static/fonts/poppins/Poppins-Light.ttf');
    font-display: swap;
  }
  @font-face {
    font-family: 'poppins-medium';
    src: url('../static/fonts/poppins/Poppins-Medium.ttf');
    font-display: swap;
  }
  body {
    font-family: 'poppins-light';
  }
  .pl {
    font-family: 'poppins-light';
  }
  .pm {
    font-family: 'poppins-medium';
  }
  .img-fit {
    width: 100%;
    height: auto;
    object-fit: cover;
  }
  .page {
    margin-bottom: 80px;
  }
</style>

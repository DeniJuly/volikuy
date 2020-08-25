<template>
    <nav class="navbar fixed-bottom">
      <div class="container justify-content-center">
        <div class="col-10 col-lg-8 col-md-8 d-flex container-item">
          <nuxt-link to="/" class="item d-flex align-items-center justify-content-center p-1" :class="{active: page == 'index'}">
            <img src="/icon/home.svg" alt="home">
            <p class="m-0 ml-1">Home</p>
          </nuxt-link>
          <nuxt-link to="/user" class="item d-flex align-items-center justify-content-center p-1" :class="{active: page == 'user'}">
            <img src="/icon/user.svg" alt="home">
            <p class="m-0 ml-1">User</p>
          </nuxt-link>
        </div>
      </div>
    </nav>
</template>

<script>
export default {
  data(){
    return {
      page: ''
    }
  },
  created(){
    this.page = this.$route.name
  }
}
</script>

<style scoped>
  .navbar {
    padding: 10px 20px!important;
    background: #FFFFFF;
    box-shadow: 0px -2px 16px rgba(0, 0, 0, 0.1);
  }
  .container-item {
    justify-content: space-between;
  }
  .navbar .item {
    width: 80px;
    height: 30px;
    border-radius: 27px;
    background: white;
    text-align: center;
  }
  .navbar .item p {
    font-size: 14px;
  }
  .navbar .item.active {
    background: #9DD0FF;
  }
</style>

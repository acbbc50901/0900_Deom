<template lang="pug">
nav
  .nav_box
    .img_box
      h1 CMYL
    .item_box
      li
        router-link(to='/login') 登入
      li
        a(href="#" @click.prevent="logout") 登出
      li
        router-link(to='/dashboard/products') 更新產品
      li
        router-link(to='/dashboard/allproducts') 總訂單
      li
        router-link(to='/dashboard/discount') 優惠券
    .search_box
      input(type="search")
</template>
<script>
export default {
  methods: {
    logout() {
      const api = `${process.env.VUE_APP_API}logout`;
      // console.log(api);
      this.$http.post(api, this.user)
        .then((res) => {
          if (res.data.success) {
            this.$router.push('/login');
          }
        });
    },
  },
};
</script>
<style lang="sass">
@import "../assets/newsass.sass"
</style>

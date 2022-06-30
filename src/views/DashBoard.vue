<template>
  <Navber></Navber>
  <div class="container-fluid mt-3 position-relative">
    <ToastMessages></ToastMessages>
    <router-view/>
  </div>
</template>
<script>
import Navber from '@/components/NevBar.vue';
import emitter from '@/methods/emitter';
import ToastMessages from '@/components/ToastMessages.vue';

export default {
  components: {
    Navber,
    ToastMessages,
  },
  created() {
    const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1');
    // console.log(token);
    this.$http.defaults.headers.common.Authorization = `${token}`;
    const api = `${process.env.VUE_APP_API}api/user/check`;
    this.$http.post(api, this.user).then((res) => {
      console.log(res);
      if (!res.data.success) {
        this.$router.push('/login');
      }
    });
  },
  provide() {
    return {
      emitter,
    };
  },
};
</script>

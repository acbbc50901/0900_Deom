<template lang='pug'>
.LoadIng(:active="isLoading")
.flex_box
  form.form_box(@submit.prevent="signIn")
    .login_box
      h1 請先登入
      .email_box
        label(for="inputEmail") Email address
        input(type="email"
            id="inputEmail"
            placeholder="Email address"
            required
            autofocus
            v-model="user.username"
            )
      .password_box
        label(for="inputPassword") PassWord
        input(type="password"
            id="inputPassword"
            placeholder="Password"
            required
            current-password= no
            v-model="user.password"
            )
      .button_box
        button(type="submit") 登入
</template>

<script>
export default {
  data() {
    return {
      user: {
        username: '',
        password: '',
        isLoading: false,
      },
    };
  },
  methods: {
    signIn() {
      const api = `${process.env.VUE_APP_API}admin/signin`;
      this.isLoading = true;
      // console.log(api);
      this.$http.post(api, this.user)
        .then((res) => {
          if (res.data.success) {
            this.isLoading = false;
            const { token, expired } = res.data;
            document.cookie = `hexToken=${token}; expires=${new Date(expired)}`;
            this.$router.push('/dashboard/products');
          } else {
            console.log('err');
          }
        });
    },
  },
};
</script>

<style lang="sass">
@mixin flex_box
  display: flex
  justify-content: center
  align-items: center
@mixin size($w,$h:$w)
  width: $w
  height: $h
*
  // border: solid 1px black
  font-family: 微軟正黑體
.flex_box
  margin-top: 20px
  display: flex
  justify-content: center
  align-items: center
  .form_box
    .login_box
      +flex_box
      padding: 100px 50px
      margin: 50px
      border-radius: 10px
      background-color: #eee
      flex-direction: column
      h1
        font-size: 24px
        font-weight: bold
        margin: 0px
      .email_box,.password_box
        margin-top: 10px
        display: flex
        flex-direction: column
        label
          margin-left: 5px
          font-weight: bold
          font-size: 14px
        input
          margin-top: 0px
          +size(350px,35px)
          border-radius: 5px
          border: solid 1px rgba(black,0.5)
          font-size: 12px
          transition: 0.3s
          &:focus
            background-color: #EEF4ED
      .button_box
        margin-top: 15px
        width: 20%
        button
          z-index: 1
          width: 100%
          padding: 5px 0px
          background-color: #333
          border-radius: 0px
          text-align: center
          font-weight: bold
          font-size: 18px
          transition: 0.5s
          position: relative
          color: #eee
          border: none
          &:after,&:before
            z-index: -1
            display: block
            content: ''
            transition: 0.5s
            transition-timing-function: easeInCubic
            background-color: transparent
            position: absolute
            +size(25%,50%)
          &:after
            border-left: 1px solid rgba(black,0.5)
            border-top: 1px solid rgba(black,0.5)
            left: -3px
            top: -3px
          &:before
            border-right: 1px solid rgba(black,0.5)
            border-bottom: 1px solid rgba(black,0.5)
            right: -3px
            bottom: -3px
          &:hover
            background-color: #fff
            color: #333
            &:after
              +size(100%,100%)
              left: 0px
              top: 0px
            &:before
              +size(100%,100%)
              right: 0px
              bottom: 0px

</style>

<template>
  <div class="login-form">
    <div class="g-form">
      <div class="g-form-line">
        <span class="g-form-label">用户邮箱：</span>
        <div class="g-form-input">
          <input type="text"
          v-model="username" placeholder="请输入用户邮箱">
        </div>
        <span class="g-form-error">{{ userErrors.errorText }}</span>
      </div>
      <div class="g-form-line">
        <span class="g-form-label">密码：</span>
        <div class="g-form-input">
          <input type="password"
          v-model="password" placeholder="请输入密码">
        </div>
        <span class="g-form-error">{{ passwordErrors.errorText }}</span>
      </div>
      <div class="g-form-line">
        <div class="g-form-btn">
          <a class="button" @click="onLogin">登录</a>
        </div>
      </div>
      <p>{{ errorText }}</p>
    </div>
  </div>
</template>

<script>
    export default {
      name: "log-from",
      data(){
        return {
          username:'',
          password:'',
          errorText:''
        }
      },
      computed: {
        userErrors() {
          let errorText, status;
          if (!/^\w+@[a-z0-9]+\.[a-z]+$/i.test(this.username)) {
            status = false;
            errorText = '不是合法的邮箱';
          } else {
            status = true;
            errorText = '';
          }
          if (!this.username) {
            status = false;
            errorText = '';
          }
          return {
            status,
            errorText
          }
        },
        passwordErrors() {
          let errorText, status;
          if (!/^\w{1,6}$/g.test(this.password)) {
            status = false;
            errorText = '密码不是1~6位';
          } else {
            status = true;
            errorText = '';
          }
          if (!this.password) {
            status = false;
            errorText = '';
          }
          return {
            status,
            errorText
          }
        }
      },
      methods: {
        onLogin() {
          if (!this.userErrors.status || !this.passwordErrors.status) {
            this.errorText = '有部分选项未通过';
          }
          this.errorText = '';
          this.$http.get('api/login')
            .then((res) => {
              console.log(res.data);
              this.$emit('has-log', res.data);
            }, (err) => {
              console.error(err);
            })
        }
      }
    }

</script>

<style scoped>

</style>

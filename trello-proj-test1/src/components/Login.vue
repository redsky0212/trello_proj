<template>
  <div class="login">
    <h2>Log in to Trello</h2>
    <form @submit.prevent="onSubmit">
      <div>
        <label for="email">Email</label>
        <input class="form-control" type="text" name="email" 
          v-model="email" autofocus placeholder="e.g., test@test.com" />
      </div>
      <div>
        <label for="password">Passwrod</label>
        <input class="form-control" type="password" 
          v-model="password" placeholder="123123" />
      </div>
      <button  class="btn" :class="{'btn-success': !invalidForm}" type="submit" 
        :disabled="invalidForm">Log In</button>
    </form>
    <p class="error" v-if="error">{{error}}</p>
  </div>
</template>

<script>
import {auth, setAuthInHeader} from '../api';

export default {
  data() {
    return {
      email: '',
      password: '',
      error: '',
      rPath: ''
    }
  },
  created(){
    this.rPath = this.$route.query.rPath || '/';
  },
  computed: {
    invalidForm() {
      return !this.email || !this.password
    }
  },
  methods: {
    onSubmit() {
      console.log(this.email, this.password)
      auth.login(this.email, this.password)
      .then(data => {
        console.log('login_api : ' +data);
        localStorage.setItem('token', data.accessToken);
        setAuthInHeader(data.accessToken);
        this.$router.push(this.rPath);
      })
      .catch(err => {
        debugger;
        console.log('login_api_err : ' +JSON.stringify(err));
        this.error = err.data.error;
      })
    }
  }
}
</script>

<style>
.login {
  width: 400px;
  margin: 0 auto; 
}
.error {
  color: #f00;
}

</style>

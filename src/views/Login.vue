<template>
  <div class="container">
    <div class="row">
      <div class="card">
        <div class="card-action">
          <form v-on:submit.prevent="submit()">
            <h4>Login</h4>
            <ul>
              <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
            </ul>
            <div>
              <label>Email:</label>
              <input type="email_address" v-model="email_address" />
            </div>
            <div>
              <label>Password:</label>
              <input type="password" v-model="password" />
            </div>
            <div class="center-align">
              <input type="submit" class="waves-effect waves-light btn" value="Login" />
            </div>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";

export default {
  data: function () {
    return {
      email_address: "",
      password: "",
      errors: [],
    };
  },
  methods: {
    submit: function () {
      var params = {
        email_address: this.email_address,
        password: this.password,
      };
      axios
        .post("/sessions", params)
        .then((response) => {
          axios.defaults.headers.common["Authorization"] = "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          this.$parent.flashMessage = "Logged in successfully!";
          this.$router.push("/");
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["Invalid email or password."];
          this.email_address = "";
          this.password = "";
        });
    },
  },
};
</script>

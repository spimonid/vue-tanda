<template>
  <div class="container">
    <div class="row">
      <div class="card">
        <div class="card-action">
          <form v-on:submit.prevent="submit()">
            <h4>Signup</h4>
            <ul>
              <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
            </ul>
            <div>
              <label>Name:</label>
              <input type="text" v-model="name" />
            </div>
            <div>
              <label>Email:</label>
              <input type="email_address" v-model="email_address" />
              <small v-if="email_address.includes('.') && !email_address.includes('@')">
                all emails must include the @ sign
              </small>
            </div>
            <div>
              <label>Password:</label>
              <input type="password" v-model="password" />
              <small>{{ 20 - password.length }} characters remaining</small>
            </div>
            <div>
              <label>Password confirmation:</label>
              <input type="password" v-model="passwordConfirmation" />
            </div>
            <input type="submit" class="waves-effect waves-light btn" value="Submit" />
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
      name: "",
      email_address: "",
      password: "",
      passwordConfirmation: "",
      errors: [],
    };
  },
  methods: {
    submit: function () {
      var params = {
        name: this.name,
        email_address: this.email_address,
        password: this.password,
        password_confirmation: this.passwordConfirmation,
      };
      axios
        .post("/users", params)
        .then((response) => {
          console.log(response.data);
          this.$router.push("/login");
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
        });
    },
  },
};
</script>

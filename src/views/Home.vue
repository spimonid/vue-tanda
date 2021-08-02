<template>
  <div class="home container" v-if="isLoggedIn()">
    <ul v-for="organization in organizations" v-bind:key="organization.id">
      <p>{{ organization.name }}</p>
      <button v-on:click="joinOrganization(organization)">Join Org</button>
    </ul>
    <h4>Create new organization</h4>
    <input v-model="name" placeholder="name of company" />
    <input v-model="hourly_rate" placeholder="hourly rate" />
    <button v-on:click="createOrganization">submit</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "Home",

  data: function () {
    return {
      organizations: [],
      joinedOrganizationsIds: [],
      name: null,
      hourly_rate: null,
    };
  },
  created: function () {
    this.indexOrganizations();
  },
  components: {},
  computed: {},
  methods: {
    createOrganization: function () {
      const params = {
        name: this.name,
        hourly_rate: this.hourly_rate,
      };
      axios
        .post(`/organizations/`, params)
        .then((response) => {
          console.log(response.data);
          this.indexOrganizations();
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(this.errors);
        });
    },
    indexOrganizations: function () {
      axios.get("/organizations").then((response) => {
        this.organizations = response.data;
        console.log(response.data);
      });
    },
    joinOrganization: function (organization) {
      const params = {
        organization_id: organization.id,
      };
      axios
        .post("/user_organizations/", params)
        .then((response) => {
          console.log(response.data);
          this.joinedOrganizationsIds.push(organization.id);
          this.$router.push(`/organizations/${organization.id}`);
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(this.errors);
        });
    },
    isLoggedIn: function () {
      return localStorage.getItem("jwt");
    },
  },
};
</script>

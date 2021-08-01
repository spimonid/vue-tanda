<template>
  <div class="home container">
    <ul v-for="organization in organizations" v-bind:key="organization.id">
      <p>{{ organization.name }}</p>
      <button v-on:click="joinOrganization(organization)">Join Org</button>
    </ul>
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
    };
  },
  created: function () {
    this.indexOrganizations();
  },
  components: {},
  computed: {},
  methods: {
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
  },
};
</script>

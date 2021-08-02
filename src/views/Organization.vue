<template>
  <div>
    <h4>{{ organization.name }}</h4>

    <ul id="nav-mobile" class="right hide-on-med-and-down">
      <li class="nav-item">
        <router-link class="nav-link" :to="`/organizations/${this.$route.params.id}/shifts`">View Shifts</router-link>
      </li>
      <li class="nav-item">
        <router-link class="nav-link" to="/">Edit</router-link>
      </li>

      <li class="nav-item" v-on:click="leaveOrganization(organization)">
        <router-link class="nav-link" to="/">Leave</router-link>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data: function () {
    return {
      organization: {},
    };
  },
  created: function () {
    this.getOrganization();
  },
  methods: {
    getOrganization: function () {
      axios.get(`/organizations/${this.$route.params.id}`).then((response) => {
        this.organization = response.data;
        console.log(response.data);
      });
    },
    leaveOrganization: function (organization) {
      const params = {
        organization_id: organization.id,
      };
      axios.post(`/user_organizations/leave`, params).then((response) => {
        console.log("LEFT ORG", response.data);
      });
    },
  },
};
</script>

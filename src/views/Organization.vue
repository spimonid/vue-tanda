<template>
  <div>
    <h4>{{ organization.name }}</h4>

    <ul id="nav-mobile" class="right hide-on-med-and-down">
      <li class="nav-item">
        <router-link class="nav-link" :to="`/organizations/${this.$route.params.id}/shifts`">View Shifts</router-link>
      </li>
      <li class="nav-item">
        <router-link class="nav-link" to="#" v-on:click.native="editMode = !editMode">Edit</router-link>
        <div v-if="editMode">
          <input v-model="name" placeholder="update name" />
          <input v-model="hourly_rate" placeholder="update hourly rate" />
          <button v-on:click="editOrganization">submit</button>
        </div>
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
      name: null,
      hourly_rate: null,
      editMode: false,
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
    editOrganization: function (organization) {
      const params = {
        id: organization.id,
        name: this.name,
        hourly_rate: this.hourly_rate,
      };

      axios
        .patch(`/organizations/${this.$route.params.id}`, params)
        .then((response) => {
          console.log(response.data);
          this.organization = response.data;
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(this.errors);
        });
      this.editMode = false;
    },
  },
};
</script>

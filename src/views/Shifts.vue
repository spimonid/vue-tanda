<template>
  <div>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Start</th>
          <th>Finish</th>
          <th>Break Time</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="shift in organization.shifts" :key="shift.id">
          <th scope="row">{{ shift.start }}</th>
          <td>{{ shift.finish }}</td>
          <td>{{ shift.break_length }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return { organization: {} };
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
  },
};
</script>

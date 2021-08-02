<template>
  <div>
    <table class="table table-striped">
      <thead>
        <tr>
          <th>Employee Name</th>
          <th>Shift Date</th>
          <th>Start Time</th>
          <th>Finish Time</th>
          <th>Break Time (minutes)</th>
          <th>Hours Worked</th>
          <th>Shift Cost</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="shift in organization.shifts" :key="shift.id">
          <td>{{ shift.user.name }}</td>
          <td>{{ shift.start.slice(0, 10) }}</td>
          <td>{{ shift.start }}</td>
          <td>{{ new Date(shift.finish) }}</td>
          <td>{{ shift.break_length }}</td>
          <td>{{ shift.hours_worked }}</td>
          <td>{{ shift.shift_cost }}</td>
        </tr>
        <tr>
          <td>{{ this.currentUser.name }}</td>
          <td><input v-model="shiftDate" placeholder="shift date" /></td>
          <td><input v-model="shiftStart" placeholder="shift start time" /></td>
          <td><input v-model="shiftFinish" placeholder="shift end time" /></td>
          <td><input v-model="breakLength" placeholder="break time" /></td>
          <td><button v-on:click="createShift">Create Shift</button></td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      organization: {},
      shiftDate: null,
      shiftStart: null,
      shiftFinish: null,
      breakLength: 0,
      currentUser: {},
    };
  },
  created: function () {
    this.getOrganization();
    this.getCurrentUser();
  },
  methods: {
    getCurrentUser: function () {
      axios.get("/current_user/").then((response) => {
        this.currentUser = response.data;
      });
    },
    getOrganization: function () {
      axios.get(`/organizations/${this.$route.params.id}`).then((response) => {
        this.organization = response.data;
      });
    },
    createShift: function () {
      axios
        .post(`/shifts`, {
          shift_date: this.shiftDate,
          start: this.shiftStart,
          finish: this.shiftFinish,
          break_length: this.breakLength,
          organization_id: this.organization.id,
          user_id: this.currentUser.id,
        })
        .then((response) => {
          console.log(response.data);
          this.getOrganization();
        })
        .catch((error) => {
          this.errors = error.response.data.errors;
          console.log(this.errors);
        });
    },
  },
};
</script>

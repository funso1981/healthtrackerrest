<template id="measurement-overview">
  <app-layout>

    <div class="card bg-light mb-3">
      <div class="card-header">
        <div class="row">
          <div class="col-6">
            User Weight Record
          </div>
          <div class="col" align="right">
            <button rel="tooltip" title="Add"
                    class="btn btn-info btn-simple btn-link"
                    @click="hideForm =!hideForm">
              <i class="fa fa-plus" aria-hidden="true"></i>
            </button>
          </div>
        </div>
      </div>
      <div class="card-body" :class="{ 'd-none': hideForm}">
        <form id="addWaist">
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-waist">Weight (KG) </span>
            </div>
            <input type="text" class="form-control" v-model="formData.waist" name="waist" placeholder="weight in KG"/>
          </div>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-waist">User Name</span>
            </div>
            <select v-model="selectedUserId">
              <option value="">Select a User</option>
              <option v-for="(user, index) in users" :value="user.id" :key="index">
                {{ user.name }}
              </option>
            </select>
          </div>
        </form>
        <button rel="tooltip" title="Update" class="btn btn-info btn-simple btn-link" @click="addWaist()">Add new Record
        </button>
      </div>
    </div>

    <div class ="table">
      <div class="row">
        <div class="col-md-12 n-bgd">
          <div class="head-name">
            <h1 class="pt-20">Weight Records</h1>
          </div>
          <div class="card tabluea sha-box-2">
            <div class="content table-responsive table-full-width">
              <table class="table table-hover table-striped">
                <thead>
                <th>Index</th>
                <th>User</th>
                <th>Date Added</th>
                <th>Weight</th>
                <th>Delete</th>
                </thead>
                <tbody>
                <tr v-for="(measurement,index) in measurements" v-bind:key="index">
                  <td>{{ index + 1 }}</td>
                  <td>{{ getUserName(measurement.userid) }}</td>
                  <td>{{ measurement.dateadded }}</td>
                  <td>{{ measurement.waist }} KG</td>
                  <td><button rel="tooltip" title="Delete" class="btn btn-info btn-simple btn-link" @click="deleteMeasurement(measurement, index)"><i class="fas fa-trash" aria-hidden="true"></i></button></td>
                </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
    <app-chart v-if="measurements.length > 0" :measurements="measurements"></app-chart>

  </app-layout>
</template>

<script>
Vue.component("measurement-overview", {
  template: "#measurement-overview",
  data: () => ({
    measurements: [],
    formData: [],
    hideForm: true,
    users: [],
    selectedUserId: "",
  }),
  created() {
    this.loadUsers();
    this.loadMeasurements();
  },

  methods: {
    getUserName(userId) {
      return this.users.find(u => u.id === userId).name
    },
    deleteMeasurement: function (measurement, index) {
      if (confirm('Are you sure you want to delete this activity? This action cannot be undone.', 'Warning')) {
        const activityId = measurement.id;
        const url = `/api/measurements/${activityId}`;
        axios.delete(url)
            .then(response =>
                //delete from the local state so Vue will reload list automatically
                this.measurements.splice(index, 1).push(response.data))
            .catch(function (error) {
              console.log(error)
            });
      }
    },
    addWaist: function () {
      const url = `/api/measurements`;
      console.log(this.formData)
      axios.post(url,
          {
            waist: this.formData.waist,
            dateadded: new Date().toISOString(),
            userid: this.selectedUserId
          })
          .then(response => {
            console.log("success")
            this.measurements.push(response.data)
            this.hideForm = true;
          })
          .catch(error => {
            console.log(error)
          })
    },
    loadUsers() {
      axios
          .get("api/users", {})
          .then((res) => {
            this.users = res.data;
          });
    },
    loadMeasurements() {
      axios
          .get("api/measurements", {})
          .then((res) => {
            this.measurements = res.data;
          });
    }
  }
});
</script>
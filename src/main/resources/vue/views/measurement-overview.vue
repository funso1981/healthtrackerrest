<template id="measurement-overview">
  <app-layout>

    <div class="card bg-light mb-3">
      <div class="card-header">
        <div class="row">
          <div class="col-6">
            User Waist Measurements
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
              <span class="input-group-text" id="input-waist">Waist</span>
            </div>
            <input type="text" class="form-control" v-model="formData.waist" name="waist" placeholder="Waist"/>
          </div>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-waist">UserId</span>
            </div>
          <select v-model="selectedUserId">
            <option value="">Select a User</option>
            <option v-for="(user, index) in users" :value="user.id" :key="index">
              {{ user.name }}
            </option>
          </select>
          </div>
        </form>
        <button rel="tooltip" title="Update" class="btn btn-info btn-simple btn-link" @click="addWaist()">Add Waist
          Reading
        </button>
      </div>
    </div>

    <app-chart v-if="measurements.length > 0" :measurements="measurements"></app-chart>
    <br/><br/>
    <div class="list-group list-group-flush">
      <div class="list-group-item d-flex align-items-start"
           v-for="(measurement,index) in measurements" v-bind:key="index">
        <div class="mr-auto p-2">
          <span>{{ getUserName(measurement.userid) }}</span>
        </div>
        <div class="mr-auto p-2">
          <span>{{ measurement.dateadded }}</span>
        </div>
        <div class="mr-auto p-2">
          <span><a :href="`/measurements/${measurement.id}`"> {{ measurement.waist }}</a></span>
        </div>
        <div class="p2">
          <a :href="`/measurements/${measurement.id}`">
            <button rel="tooltip" title="Update" class="btn btn-info btn-simple btn-link">
              <i class="fa fa-pencil" aria-hidden="true"></i>
            </button>
          </a>
          <!--<button rel="tooltip" title="Delete" class="btn btn-info btn-simple btn-link"
                  @click="deleteItem(item, index)">
            <i class="fas fa-trash" aria-hidden="true"></i>
          </button>-->
        </div>
      </div>
    </div>

  </app-layout>
</template>

<style>
*c

</style>
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
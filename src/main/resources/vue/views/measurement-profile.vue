<template id="user-profile">
  <app-layout>
    <div v-if="noMeasurementFound">
      <p> We're sorry, we were not able to retrieve this measurement.</p>
      <p> View <a :href="'/users'">all users</a>.</p>
    </div>
    <div class="card bg-light mb-3" v-if="!noMeasurementFound">
      <div class="card-header">
        <div class="row">
          <div class="col-6"> Measurement Profile </div>
          <div class="col" align="right">
            <button rel="tooltip" title="Update"
                    class="btn btn-info btn-simple btn-link"
                    @click="updateMeasurement()">
              <i class="far fa-save" aria-hidden="true"></i>
            </button>
            <button rel="tooltip" title="Delete"
                    class="btn btn-info btn-simple btn-link"
                    @click="deleteMeasurement()">
              <i class="fas fa-trash" aria-hidden="true"></i>
            </button>
          </div>
        </div>
      </div>
      <div class="card-body">
        <form>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-waist-measurement">Waist in Inches</span>
            </div>
            <input type="number" class="form-control" v-model="waist.id" name="id" readonly placeholder="Id"/>
          </div>
        </form>
      </div>
      <div class="card-footer text-left">
        <p  v-if="measurements.length == 0"> No measurement yet...</p>
        <p  v-if="measurements.length > 0"> Measurement so far...</p>
        <ul>
          <li v-for="measurement in measurements">
            {{ measurement.description }}
          </li>
        </ul>
      </div>
    </div>
  </app-layout>
</template>

<script>
Vue.component("user-profile", {
  template: "#user-profile",
  data: () => ({
    user: null,
    noUserFound: false,
    activities: [],
  }),
  created: function () {
        const userId = this.$javalin.pathParams["user-id"];
        const url = `/api/users/${userId}`
        axios.get(url)
            .then(res => this.user = res.data)
            .catch(error => {
              console.log("No user found for id passed in the path parameter: " + error)
              this.noUserFound = true
            })
        axios.get(url + `/activities`)
            .then(res => this.activities = res.data)
            .catch(error => {
              console.log("No activities added yet (this is ok): " + error)
            })
      },
  methods: {
    updateUser: function () {
      const userId = this.$javalin.pathParams["user-id"];
      const url = `/api/users/${userId}`
      axios.patch(url,
          {
            name: this.user.name,
            email: this.user.email
          })
          .then(response =>
              this.user.push(response.data))
          .catch(error => {
            console.log(error)
          })
      alert("User updated!")
    },
    deleteUser: function () {
      if (confirm("Do you really want to delete?")) {
        const userId = this.$javalin.pathParams["user-id"];
        const url = `/api/users/${userId}`
        axios.delete(url)
            .then(response => {
              alert("User deleted")
              //display the /users endpoint
              window.location.href = '/users';
            })
            .catch(function (error) {
              console.log(error)
            });
      }
    }
  }
});
</script>
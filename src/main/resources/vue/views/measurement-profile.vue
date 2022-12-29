<template id="user-profile">
  <app-layout>
    <div v-if="noMeastFound">
      <p> We're sorry, we were not able to retrieve the measurement details of this user.</p>
      <p> View <a :href="'/measurements'">User Measurements</a>.</p>
    </div>
    <div class="card bg-light mb-3" v-if="!noMeastFound">
      <div class="card-header">
        <div class="row">
          <div class="col-6"> Measurement Profile </div>
          <div class="col" align="right">
            <button rel="tooltip" title="Update"
                    class="btn btn-info btn-simple btn-link"
                    @click="updateMeast()">
              <i class="far fa-save" aria-hidden="true"></i>
            </button>
            <button rel="tooltip" title="Delete"
                    class="btn btn-info btn-simple btn-link"
                    @click="deleteMeast()">
              <i class="fas fa-trash" aria-hidden="true"></i>
            </button>
          </div>
        </div>
      </div>
      <div class="card-body">
        <form>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-user">User ID</span>
            </div>
            <input type="number" class="form-control" v-model="user.id" name="id" readonly placeholder="Id"/>
          </div>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-user-waist">Waist</span>
            </div>
            <input type="text" class="form-control" v-model="measurement.waist" name="waist" placeholder="Waist"/>
          </div>
        </form>
      </div>
      <div class="card-footer text-left">
        <p  v-if="measurements.length == 0"> No measurements yet...</p>
        <p  v-if="measurements.length > 0"> Measurements tracked so far...</p>
        <ul>
          <li v-for="measurement in measurements">
            Your waist measurement is {{ measurement.waist }} inches
          </li>
        </ul>
      </div>
    </div>
  </app-layout>
</template>

<script>
Vue.component("measurement-profile", {
  template: "#measurement-profile",
  data: () => ({
    user: null,
    noUserFound: false,
    measurements: [],
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
        axios.get(url + `/measurements`)
            .then(res => this.measurements = res.data)
            .catch(error => {
              console.log("No measurements added yet (this is ok): " + error)
        })
      },
  methods: {
    updateUser: function () {
      const userId = this.$javalin.pathParams["user-id"];
      const url = `/api/users/${userId}`
      axios.patch(url,
          {
            name: this.user.name,
          })
          .then(response =>
              this.user.push(response.data))
          .catch(error => {
            console.log(error)
          })
      alert("User updated!")
    },
    deleteMeast: function () {
      if (confirm("Do you really want to delete?")) {
        const userId = this.$javalin.pathParams["user-id"];
        const url = `/api/measurements/${userId}`
        axios.delete(url)
            .then(response => {
              alert("Measurement deleted")
              //display the /users endpoint
              window.location.href = '/measurements';
            })
            .catch(function (error) {
              console.log(error)
            });
      }
    }
  }
});
</script>
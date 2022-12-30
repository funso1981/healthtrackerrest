<template id="activity-overview">
  <app-layout>
    <div class="card bg-light mb-3">
      <div class="card-header">
        <div class="row">
          <div class="col-6">
            Activities
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
        <form id="addActivity">
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-desc">Description</span>
            </div>
            <input type="text" class="form-control" v-model="formData.description" name="description" placeholder="Description"/>
          </div>

          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-duration ">Duration</span>
            </div>
            <input type="text" class="form-control" v-model="formData.duration" name="duration" placeholder="Duration"/>
          </div>

          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-calories">Calories</span>
            </div>
            <input type="text" class="form-control" v-model="formData.calories" name="calories" placeholder="Calories"/>
          </div>

          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-calories">Username</span>
            </div>
            <select v-model="selectedUserId" >
              <option value="">Select a User</option>
              <option
                  v-for="(user, index) in users"
                  :value="user.id"
                  :key="index"
              >
                {{ user.name }}
              </option>
            </select>
          </div>



        </form>
        <button rel="tooltip" title="Update" class="btn btn-info btn-simple btn-link" @click="addActivity()">Add Activity</button>
      </div>
    </div>
    <div class ="table">
      <div class="row">
        <div class="col-md-12 n-bgd">
          <div class="head-name">
            <h1 class="pt-20">Activities</h1>
          </div>
          <div class="card tabluea sha-box-2">
            <div class="content table-responsive table-full-width">
              <table class="table table-hover table-striped">
                <thead>
                <th>Index</th>
                <th>User</th>
                <th>Activity</th>
                <th>Duration</th>
                <th>Calories Burned</th>
                <th>Delete</th>
                </thead>
                <tbody>
                <tr v-for="(activity,index) in activities" v-bind:key="index">
                  <td>{{ index + 1 }}</td>
                  <td>{{ getUserName(activity.userId) }}</td>
                  <td>{{ activity.description }}</td>
                  <td>{{ activity.duration }}</td>
                  <td>{{ activity.calories }}</td>
                  <td><button rel="tooltip" title="Delete" class="btn btn-info btn-simple btn-link" @click="deleteActivity(activity, index)"><i class="fas fa-trash" aria-hidden="true"></i></button></td>
                </tr>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>

  </app-layout>
</template>
<script>
Vue.component("activity-overview", {
  template: "#activity-overview",
  data: () => ({
    activities: [],
    formData: [],
    hideForm :true,

    users: [],
    selectedUserId: "",
  }),
  created() {
    this.fetchActivities();
    this.loadUsers();
  },
  methods: {
    getUserName(userId) {
      return this.users.find(u => u.id === userId).name
    },
    fetchActivities: function () {
      axios.get("/api/activities")
          .then(res => this.activities = res.data)
          .catch(() => alert("Error while fetching activities"));
    },
    deleteActivity: function (activity, index) {
      if (confirm('Are you sure you want to delete this activity? This action cannot be undone.', 'Warning')) {
        //activity confirmed delete
        const activityId = activity.id;
        const url = `/api/activities/${activityId}`;
        axios.delete(url)
            .then(response =>
                //delete from the local state so Vue will reload list automatically
                this.activities.splice(index, 1).push(response.data))
            .catch(function (error) {
              console.log(error)
            });
      }
    },
    addActivity: function (){
      const url = `/api/activities`;
      axios.post(url,
          {
            description: this.formData.description,
            duration: this.formData.duration,
            calories: this.formData.calories,
            started: new Date().toISOString(),
            userId: this.selectedUserId
          })
          .then(response => {
            this.activities.push(response.data)
            this.hideForm= true;
          })
          .catch(error => {
            console.log(error)
          })
    },

    loadUsers() {
      axios
          .get("api/users", {
          })
          .then((res) => {
            this.users = res.data;
          });
      console.log(this.activities)
    }
  }
});
</script>
<!-- the "home-page" element is passed as a parameter to VueComponent in the JavalinConfig file -->
<template id="home-page">
  <app-layout>
    <div class="row">
      <div class="col">
        <div class="card">
          <h5 class="card-header">Registered Users</h5>
          <div class="card-body">
            <h5 class="card-title">{{users.length}} users</h5>
            <a href="/users" class="btn btn-success">More Details...</a>
          </div>
        </div>
      </div>
      <div class="col">
        <div class="card">
          <h5 class="card-header">Total Activities</h5>
          <div class="card-body">
            <h5 class="card-title">{{activities.length}} activities</h5>
            <a href="/activities" class="btn btn-success">More Details...</a>
          </div>
        </div>
      </div>
      <div class="col">
        <div class="card">
          <h5 class="card-header">Total Items</h5>
          <div class="card-body">
            <h5 class="card-title">{{items.length}} items</h5>
            <a href="/items" class="btn btn-success">More Details...</a>
          </div>
        </div>
      </div>
      <div class="col" >
        <div class="card">
          <h5 class="card-header">Measurements</h5>
          <div class="card-body" >
            <h5 class="card-title">{{measurements.length}} measurements</h5>
            <a href="/measurements" class="btn btn-success">More Details...</a>
          </div>
        </div>
      </div>
    </div>
  </app-layout>
</template>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'montserrat', sans-serif;
}

body {
  background-color: #eee;
}

main {
  padding: 1.5rem;
}

h1 {
  font-size: 2em;
  text-align: center;
  margin-bottom: 2rem;
}

.col {
  margin-bottom: 1rem;
  color: #888;
  font-weight: 400;
}
</style>

<script>

Vue.component('home-page',
    {
      template: "#home-page",
      data: () => ({
        users: [],
        activities: [],
        items: [],
        measurements: []
      }),
      components: {

      },
      created() {
        axios.get("/api/users")
            .then(res => this.users = res.data)
            .catch(() => alert("Error while fetching users"));
        axios.get("/api/activities")
            .then(res => this.activities = res.data)
            .catch(() => alert("Error while fetching activities"));
        axios.get("/api/items")
            .then(res => this.items = res.data)
            .catch(() => alert("Error while fetching items"));
        axios.get("/api/measurements")
            .then(res => this.measurements = res.data)
            .catch(() => alert("Error while fetching measurements"));
      }
    });
</script>
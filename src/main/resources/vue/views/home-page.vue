<!-- the "home-page" element is passed as a parameter to VueComponent in the JavalinConfig file -->
<template id="home-page">
  <app-layout>
    <div class="container-fluid">
      <div class="row">
        <div class="dash-header">
          <div class="col-md-12">
            <a href="/users">
              <div class="sha-box dash-detail gb-rounded gb-bordered hover-fill reverse hover-slide">
                <i class="dash-icon fa fa-users"></i>
                <p class="dash-detail-h2">Total Users</p>
                <p class="dash-detail-p">{{users.length}} users</p>
              </div>
            </a>
            <a href="/activities">
              <div class="sha-box dash-detail gb-rounded gb-bordered hover-fill reverse hover-slide">
                <i class="dash-icon fa fa-cogs"></i>
                <p class="dash-detail-h2">Total Activities</p>
                <p class="dash-detail-p">{{activities.length}} Activities</p>
              </div>
            </a>
            <a href="/items">
              <div class="sha-box dash-detail gb-rounded gb-bordered hover-fill reverse hover-slide">
                <i class="dash-icon fa fa-briefcase"></i>
                <p class="dash-detail-h2">Exercises</p>
                <p class="dash-detail-p">{{items.length}} Exercises</p>
              </div>
            </a>
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

.sha-box {
  box-shadow: rgba(50, 50, 93, 0.25) 0px 13px 27px -5px,
  rgba(0, 0, 0, 0.3) 0px 8px 16px -8px !important;
}
.sha-box-2 {
  box-shadow: rgba(0, 0, 0, 0.07) 0px 1px 2px, rgba(0, 0, 0, 0.07) 0px 2px 4px,
  rgba(0, 0, 0, 0.07) 0px 4px 8px, rgba(0, 0, 0, 0.07) 0px 8px 16px,
  rgba(0, 0, 0, 0.07) 0px 16px 32px, rgba(0, 0, 0, 0.07) 0px 32px 64px !important;
}
.dash-header {
  margin-left: 0;
  height: 350px;
  width: 100%;
  border: 1px solid #AFDBFF;
  border-top: none !important;
}

.dash-detail {
  display: inline-block;
  height: 250px;
  width: 27%;
  margin: 2% 3%;
  cursor: pointer;
  font-size: 18px;
  border-radius: 8px;
  padding: 6px 20px;
  position: relative;
  border: 4px solid transparent;
  background-clip: padding-box;
  transition: 0.5s all;
}

.dash-detail:after {
  transition: 0.5s all;
  position: absolute;
  top: -4px;
  left: -4px;
  right: -4px;
  bottom: -4px;
  content: "";
  z-index: -1;
  border-radius: 4px;
  background-image: linear-gradient(
      135deg,
      #AFDBFF 0%,
      #AFDBFF 18%,
      #8DCCFF 18%,
      #8DCCFF 28%,
      #38A6FF 28%,
      #38A6FF 72%,
      #0061AF 72%,
      #0061AF 100%
  );
}

.dash-icon {
  text-align: center;
  display: block;
  font-size: 40px;
  color: #133e61;
  margin: 5px auto;
  padding-right: 50px;
}

.dash-detail-h2 {
  font-size: 40px;
  margin: 10px;
  font-weight: 300;
  text-align: left;
}

.dash-detail-p {
  font-size: 20px;
  margin: 10px;
  text-align: left;
}

.gb-rounded {
  border-radius: 50px;
}

.gb-rounded.gb-bordered:after {
  border-radius: 50px;
}

.hover-fill {
  color: #f7f7f7;
  transition: 0.6s all;
}

.hover-fill:hover {
  background-color: transparent;
}

.hover-fill.reverse {
  background-color: transparent;
  color: #f7f7f7;
}

.hover-fill.reverse:hover {
  background-color: #f9f6f1;
  color: #4d4a21;
}

.hover-slide {
  transition: 0.6s all;
}

.hover-slide:after {
  background-size: 200% 100%;
  background-position: 0% 0;
}

.hover-slide:hover:after {
  background-position: 100% 0;
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
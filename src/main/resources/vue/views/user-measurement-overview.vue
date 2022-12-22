<template id="user-measurement-overview">
  <div>
    <app-layout>
    <h3>Measurements list </h3>
    </app-layout>
    <ul>
      <li v-for="measurement in measurements">
        {{measurement.id}}: {{measurement.waist}}
      </li>
    </ul>
  </div>
</template>

<script>
Vue.component("user-measurement-overview",{
  template: "#user-measurement-overview",
  data: () => ({
    measurements: [],
  }),
  created() {
    const userId = this.$javalin.pathParams["user-id"];
    axios.get(`/api/users/${userId}/measurements`)
        .then(res => this.measurements = res.data)
        .catch(() => alert("Error while fetching measurements"));
  }
});
</script>
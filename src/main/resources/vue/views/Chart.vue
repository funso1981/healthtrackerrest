<template id="app-chart">
  <canvas id="myChart" style="width:80%;"></canvas>
</template>

<script>
Vue.component('app-chart',
    {
      template: "#app-chart",
      props: {
        measurements: Array
      },
      mounted() {
        const labels = ["user"];
        const values = [0];

        this.measurements.forEach(measurement => {
          let user = measurement.userid;
          labels.push(user);
          values.push(measurement.waist);
        })

        new Chart("myChart", {
          type: "bar",
          data: {
            labels: labels,
            datasets: [{
              label: 'User Weight Record',
              backgroundColor: "rgba(175, 219, 255, 0.8)",
              borderColor: "rgba(175, 219, 255, 1)",
              data: values
            }]
          },
          options: {
            label: ""
          }
        });
      },
      data: () => ({})
    });

</script>
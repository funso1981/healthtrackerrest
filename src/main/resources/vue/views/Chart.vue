<template id="app-chart">
  <canvas id="myChart" style="width:100%;max-width:700px"></canvas>
</template>

<script>
Vue.component('app-chart',
    {
      template: "#app-chart",
      props: {
        measurements: Array
      },
      mounted() {

        const labels = [];
        const values = [];

        this.measurements.forEach(measurement => {
          let date = measurement.dateadded;
          labels.push(new Date(date).toDateString());
          values.push(measurement.waist);
        })

        new Chart("myChart", {
          type: "line",
          data: {
            labels: labels,
            datasets: [{
              label: 'User Waist Measurements',
              backgroundColor: "rgba(255, 105, 180, 0.2)",
              borderColor: "rgba(255, 105, 180, 1)",
              data: values
            }]
          },
          options: {
            label: "FUnso"
          }
        });
      },
      data: () => ({})
    });

</script>
<template id="user-item-overview">
  <div>
    <app-layout>
    <h3>Items list </h3>
    </app-layout>
    <ul>
      <li v-for="item in items">
        {{item.id}}: {{item.name}} for {{item.quantity}} 
      </li>
    </ul>
  </div>
</template>

<script>
Vue.component("user-item-overview",{
  template: "#user-item-overview",
  data: () => ({
    items: [],
  }),
  created() {
    const userId = this.$javalin.pathParams["user-id"];
    axios.get(`/api/users/${userId}/items`)
        .then(res => this.items = res.data)
        .catch(() => alert("Error while fetching items"));
  }
});
</script>
<template id="item-overview">
  <app-layout>
    <div class="card bg-light mb-3">
      <div class="card-header">
        <div class="row">
          <div class="col-6">
            Exercises
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
        <form id="addItem">
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-name">Name</span>
            </div>
            <input type="text" class="form-control" v-model="formData.name" name="name" placeholder="Name of Excercise"/>
          </div>

          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-quantity ">Reps of Excercise</span>
            </div>
            <input type="text" class="form-control" v-model="formData.quantity" name="quantity" placeholder="Reps"/>
          </div>

          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-waist">User Name</span>
            </div>
            <select v-model="selectedUserId">
              <option value="">Select a User</option>
              <option v-for="(user, index) in users" :value="user.id" :key="index">
                {{ user.name }}
              </option>
            </select>
          </div>
        </form>
        <button rel="tooltip" title="Update" class="btn btn-info btn-simple btn-link" @click="addItem()">Add Excercise</button>
      </div>
    </div>

    <div class ="table">
      <div class="row">
        <div class="col-md-12 n-bgd">
          <div class="head-name">
            <h1 class="pt-20">Exercises</h1>
          </div>
          <div class="card tabluea sha-box-2">
            <div class="content table-responsive table-full-width">
              <table class="table table-hover table-striped">
                <thead>
                <th>Index</th>
                <th>Name of Exercises</th>
                <th>Date Added</th>
                <th>Sets</th>
                <th>Delete</th>
                </thead>
                <tbody>
                <tr v-for="(item,index) in items" v-bind:key="index">
                  <td>{{ index + 1 }}</td>
                  <td>{{ item.name }}</td>
                  <td>{{ item.dateadded }}</td>
                  <td>{{ item.quantity }} Reps</td>
                  <td><button rel="tooltip" title="Delete" class="btn btn-info btn-simple btn-link" @click="deleteItem(item, index)"><i class="fas fa-trash" aria-hidden="true"></i></button></td>
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
Vue.component("item-overview", {
  template: "#item-overview",
  data: () => ({
    items: [],
    formData: [],
    hideForm :true,

    users: [],
    selectedUserId: "",
  }),
  created() {
    this.fetchItems();
    this.loadUsers();
  },
  methods: {
    fetchItems: function () {
      axios.get("/api/items")
          .then(res => this.items = res.data)
          .catch(() => alert("Error while fetching items"));
    },
    deleteItem: function (item, index) {
      if (confirm('Are you sure you want to delete this item? This action cannot be undone.', 'Warning')) {
        //item confirmed delete
        const itemId = item.id;
        const url = `/api/items/${itemId}`;
        axios.delete(url)
            .then(response =>
                //delete from the local state so Vue will reload list automatically
                this.items.splice(index, 1).push(response.data))
            .catch(function (error) {
              console.log(error)
            });
      }

    },
    addItem: function (){
      const url = `/api/items`;
      console.log(this.formData)
      axios.post(url,
          {
            name: this.formData.name,
            quantity: this.formData.quantity,
            dateadded: new Date().toISOString(),
            userid: this.selectedUserId
          })
          .then(response => {
            console.log("success")
            this.items.push(response.data)
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
    },
  }
});
</script>
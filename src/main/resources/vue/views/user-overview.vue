<template id="user-overview">
  <app-layout>
    <div class="card bg-light mb-3">
      <div class="card-header">
        <div class="row">
          <div class="col-6">Add New User</div>
          <div class="col" align="right">
            <button
                rel="tooltip"
                title="Add"
                class="btn btn-info btn-simple btn-link"
                @click="hideForm = !hideForm"
            >
              <i class="fa fa-plus" aria-hidden="true"></i>
            </button>
          </div>
        </div>
      </div>
      <div class="card-body" :class="{ 'd-none': hideForm }">
        <form id="addUser">
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-user-name">Name</span>
            </div>
            <input
                type="text"
                class="form-control"
                v-model="formData.name"
                name="name"
                placeholder="Name"
            />
          </div>
          <div class="input-group mb-3">
            <div class="input-group-prepend">
              <span class="input-group-text" id="input-user-email">Email</span>
            </div>
            <input
                type="email"
                class="form-control"
                v-model="formData.email"
                name="email"
                placeholder="Email"
            />
          </div>
        </form>
        <button
            rel="tooltip"
            title="Update"
            class="btn btn-info btn-simple btn-link"
            @click="addUser()"
        >
          Add User
        </button>
      </div>
    </div>
    <div class ="table">

      <div class="row tabbb">
        <div class="col-md-12 n-bgd">
          <div class="head-name">
            <h1 class="pt-20">Users</h1>
          </div>
          <div class="card tabluea sha-box-2">
            <div class="content table-responsive table-full-width">
              <table class="table table-hover table-striped">
                <thead>
                <th>Index</th>
                <th>Name</th>
                <th>Email</th>
                <th>Edit</th>
                <th>Delete</th>
                </thead>
                <tbody>
                <tr v-for="(user, index) in users" v-bind:key="index">
                  <td>{{ index + 1 }}</td>
                  <td><a :href="`/users/${user.id}`">{{ user.name }}</a></td>
                  <td>{{ user.email }}</td>
                  <td><a :href="`/users/${user.id}`"><button rel="tooltip" title="Update" class="btn btn-info btn-simple btn-link"><i class="fa fa-pencil" aria-hidden="true"></i></button></a></td>
                  <td><button rel="tooltip" title="Delete" class="btn btn-info btn-simple btn-link" @click="deleteUser(user, index)"><i class="fas fa-trash" aria-hidden="true"></i></button></td>
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
Vue.component("user-overview", {
  template: "#user-overview",
  data: () => ({
    users: [],
    formData: [],
    hideForm: true,
  }),
  created() {
    this.fetchUsers();
  },
  methods: {
    fetchUsers: function () {
      axios
          .get("/api/users")
          .then((res) => (this.users = res.data))
          .catch(() => alert("Error while fetching users"));
    },
    deleteUser: function (user, index) {
      if (
          confirm(
              "Are you sure you want to delete this user? This action cannot be undone.",
              "Warning"
          )
      ) {
        //user confirmed delete
        const userId = user.id;
        const url = `/api/users/${userId}`;
        axios
            .delete(url)
            .then((response) =>
                //delete from the local state so Vue will reload list automatically
                this.users.splice(index, 1).push(response.data)
            )
            .catch(function (error) {
              console.log(error);
            });
      }
    },
    addUser: function () {
      const url = `/api/users`;
      axios
          .post(url, {
            name: this.formData.name,
            email: this.formData.email,
          })
          .then((response) => {
            this.users.push(response.data);
            this.hideForm = true;
          })
          .catch((error) => {
            console.log(error);
          });
    },
  },
});
</script>
<style>
.table-striped>tbody>tr:nth-of-type(odd) {
  --bs-table-accent-bg: #AFDBFF !important;
}
</style>
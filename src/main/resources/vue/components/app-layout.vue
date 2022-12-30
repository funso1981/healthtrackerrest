<template id="app-layout">
  <div class="app-layout">
    <div class="container">
      <!-- Start of navbar -->
      <nav class="navbar navbar-expand-lg navbar-light">
        <button
            class="navbar-toggler"
            type="button"
            data-toggle="collapse"
            data-target="#navbarSupportedContent"
            aria-controls="navbarSupportedContent"
            aria-expanded="false"
            aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarSupportedContent">
          <ul class="navbar-nav mr-auto">
            <li class="nav-item active">
              <a class="nav-link" href="/">
                <i class="fa icon fa-bars"></i>
                Dashboard <span class="sr-only">(current)</span>
              </a>
            </li>
            <li class="nav-item active">
              <a class="nav-link" href="/users">
                <i class="fa icon fa-users"></i>
                Users <span class="sr-only">(current)</span>
              </a>
            </li>
            <li class="nav-item active">
              <a class="nav-link" href="/activities"
              ><i class="fa icon  fa-cogs"></i>Activities <span class="sr-only">(current)</span></a
              >
            </li>
            <li class="nav-item active">
              <a class="nav-link" href="/items"
              ><i class="fa icon fa-briefcase"></i>Exercises <span class="sr-only">(current)</span></a
              >
            </li>
            <li class="nav-item active">
              <a class="nav-link" href="/measurements"
              ><i class="fa icon fa-area-chart"></i>Weight Tracking <span class="sr-only">(current)</span></a
              >
            </li>
          </ul>
        </div>
      </nav>
      <!--End of nav bar-->
      <!--Start of main content area-->
      <div class="content mt-3">
        <div class="container-fluid">
          <slot></slot>
        </div>
      </div>
      <!--End of main content area-->
    </div>
  </div>
</template>

<script>
Vue.component("app-layout", { template: "#app-layout" });
</script>

<style>
.navbar {
  background-color: #e3f2fd;
}
.icon {
  color: #062f44;
  margin: 5px auto;
  padding-right: 10px;
}
.nav-link {
  padding: 20px !important;
}
</style>

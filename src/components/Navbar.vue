<script lang="ts">
export default {
  name: 'Navbar',
}
</script>

<template>
  <div class="wrapper">
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
      <div class="container">
        <RouterLink class="navbar-brand" to="/"> Poralis</RouterLink>
        <button
          class="navbar-toggler"
          type="button"
          data-bs-toggle="collapse"
          data-bs-target="#navbarNav"
          aria-controls="navbarNav"
          aria-expanded="false"
          aria-label="Toggle navigation"
        >
          <span class="navbar-toggler-icon"></span>
        </button>

        <div class="collapse navbar-collapse" id="navbarNav">
          <ul class="navbar-nav">
            <li class="nav-item">
              <RouterLink class="nav-link" to="/game"> ゲーム一覧 </RouterLink>
            </li>
            <li class="nav-item">
              <RouterLink class="nav-link" to="/game-manage"> ゲーム管理 </RouterLink>
            </li>
          </ul>
        </div>
      </div>
    </nav>
  </div>
</template>

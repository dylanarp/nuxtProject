<template>
    <div class="nav">
      <client-only>
        <b-nav>
          <b-nav-item active>
            <nuxt-link to="/">Home</nuxt-link>
          </b-nav-item>

          <b-nav-item>
            <nuxt-link to="/Create">Create</nuxt-link>
          </b-nav-item>
        </b-nav>
      </client-only>
    </div>
</template>

<style scoped>
  .nav {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 500px;
    margin: 25px auto 25px auto;
    background: #F5F5F5;
  }
</style>

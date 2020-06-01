<template>
  <div id="app">
    <b-navbar class="is-primary">
      <template slot="brand">
        <b-navbar-item class="title is-3" tag="router-link" :to="{ path: '/' }">Doggy</b-navbar-item>
      </template>
      <template slot="end">
        <b-navbar-item href="/image-search">Search</b-navbar-item>
        <b-navbar-item href="/about">About</b-navbar-item>
      </template>
    </b-navbar>
    <!-- <nav class="navbar is-primary" role="navigation" aria-label="main navigation">
      <div class="navbar-brand">
        <router-link class="navbar-item" to="/">
          <h1 class="title is-3">Doggy</h1>
        </router-link>
      </div>
      <div class="navbar-end">
        <router-link class="navbar-item has-text-white" to="/about">About</router-link>
        <router-link class="navbar-item has-text-white" to="/search">Search</router-link>
        <router-link class="navbar-item has-text-white" to="/image-search">Image Search</router-link>
      </div>
    </nav>-->
    <div class="container mt-2">
      <router-view />
    </div>
  </div>
</template>

<style lang="scss">
</style>

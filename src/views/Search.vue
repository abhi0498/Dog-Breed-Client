<template>
  <div>
    <h1 class="title is-1">Search By Text</h1>
    <form @submit.prevent="search">
      <b-field>
        <b-input style="width:40%" placeholder="Search..." type="search" icon="magnify"></b-input>
        <p class="control">
          <button class="button is-primary" outlined>Search</button>
        </p>
      </b-field>
    </form>
  </div>
</template>

<script>
export default {
  data: () => ({
    term: ""
  })
};
</script>

<style></style>

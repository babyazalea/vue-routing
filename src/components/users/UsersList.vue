<template>
  <button @click="confirmInput">confirm</button>
  <button @click="saveChanges">Save Changes</button>
  <ul>
    <user-item
      v-for="user in users"
      :key="user.id"
      :name="user.fullName"
      :role="user.role"
    ></user-item>
  </ul>
</template>

<script>
import UserItem from './UserItem.vue';

export default {
  data() {
    return { changeSaved: false };
  },
  components: {
    UserItem
  },
  inject: ['users'],
  methods: {
    confirmInput() {
      this.$router.push('/teams');
    },
    saveChanges() {
      this.changeSaved = true;
    }
  },
  beforeRouteEnter(to, from, next) {
    console.log('userList cmp before enter');
    console.log(to, from);
    next();
  },
  beforeRouteLeave(to, from, next) {
    console.log('userList cmp beforeRouteLeave');
    console.log(to, from);
    if (this.changeSaved) {
      next();
    } else {
      const userWantToLeave = confirm('Are you sure? you got unsaved Chages!');
      next(userWantToLeave);
    }
  },
  unmounted() {
    console.log('unmounted');
  }
};
</script>

<style scoped>
ul {
  list-style: none;
  margin: 2rem auto;
  max-width: 20rem;
  padding: 0;
}
</style>

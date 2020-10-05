<template>
  <div id="app">
    @{{ user.username }} - {{ fullName }}
    <strong>Followers:</strong>  {{ followers }}
    <!-- @event = v-on:event directive to listen to DOM events and run some JavaScript when they’re triggered. -->
    <button @click="followUser" >
      Follow
    </button>
  </div>
</template>

<script>

export default {
  name: 'App',
  // similar to react states
  data() {
    return {
      followers: 0,
      user: {
        id: 1,
        username: '_MitchellRomney',
        firstName: 'Mitchell',
        lastName: 'Romney',
        email: 'mitchellromney@theearththissqure.com',
        isAdmin: true
      }
    }
  },
  // watch data and do something when it changes
  // Statename (newState, oldState)
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`)
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`
    }
  },
  // create methods here
  methods: {
    followUser() {
      this.followers++
    }
  },
  // lifecycle methods bzw. hooks in Vue
  mounted() {
    this.followUser();
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
  display: flex;
  flex-direction: column;
  text-align: center;
}
</style>

<template>
    <div class="user-profile">
        <div class="user-profile__user-panel">
            <h1 class="user-profile__username">@{{ user.username }}</h1>
            <!-- v-if: render div if condition is met -->
            <div class="user-profile__admin-badge" v-if="user.isAdmin">
                Admin
            </div>
            <div class="user-profile__follower-count">
                <strong>Followers: </strong> {{ followers }}
            </div>
            <form action="" class="user-profile__create-twoot" @submit.prevent="createNewTwoot">
                <label for="newTwoot"><strong>New Twoot</strong></label>
                <!-- v-model: Two way input binding. Saves input into the data set of v-model -->
                <textarea name="" id="newTwoot" cols="" rows="4" v-model="newTwootContent" />

                <div class="user-profile__create-twoot-type">
                    <label for="newTwootType"><strong>Type</strong></label>
                    <select name="" id="newTwootType" v-model="selectedTwootType">
                        <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
                            {{ option.name }}
                        </option>
                    </select>
                </div>

                <button>
                    Twoot!
                </button>
            </form>
        </div>
        <div class="user-profile__twoots-wrapper">
            <TwootItem
                v-for="twoot in user.twoots"
                :key="twoot.id"
                :username="user.username"
                :twoot="twoot"
                @favourite="toggleFavourite"
            />
        </div>
    </div>
</template>

<script>
import TwootItem from "./TwootItem";

export default {
    name: 'UserProfile',
    components: { TwootItem },
  // similar to react states
  data() {
    return {
      newTwootContent: '',
      selectedTwootType: 'instant',
      twootTypes: [
          { value: 'draft', name: 'Draft' },
          { value: 'instant', name: 'Instant Twoot' }
      ],
      followers: 0,
      user: {
        id: 1,
        username: '_MitchellRomney',
        firstName: 'Mitchell',
        lastName: 'Romney',
        email: 'mitchellromney@theearththissqure.com',
        isAdmin: true,
        twoots: [
           { id: 1, content: 'Twotter is Amazing!'},
           { id: 2, content: "Don't forget to subscribe to The Earth is Square!"}
        ]
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
    },
    toggleFavourite(id) {
        console.log(`Favourited Tweet #${id}`)
    },
    createNewTwoot() {
        if (this.newTwootContent && this.selectedTwootType !== 'draft') {
            this.user.twoots.unshift({
                id: this.user.twoots.length + 1,
                content: this.newTwootContent
            })
        }
    }
  },
  // lifecycle methods bzw. hooks in Vue
  mounted() {
    this.followUser();
  }
}
</script>

<style>
    .user-profile {
        display: grid;
        grid-template-columns: 1fr 3fr;
        width: 100%;
        padding: 50px 5%;
    }

    .user-profile__user-panel {
        display: flex;
        flex-direction: column;
        margin-right: 50px;
        padding: 20px;
        background-color: #fff;
        border-radius: 5px;
        border: 1px solid #DFE3E8;
    }

    h1 {
        margin: 0;
    }

    .user-profile__admin-badge {
        background: rebeccapurple;
        color: white;
        border-radius: 5px;
        padding: 0 10px;
        margin-right: auto;
        font-weight: bold;
    }

    .user-profile__create-twoot {
        padding-top: 20px;
        display: flex;
        flex-direction: column;
    }
</style>
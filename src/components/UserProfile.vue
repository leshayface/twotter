<template>
  <div class="container">
    <p><strong>@{{user.username}}</strong> - {{fullName}}</p>
    <div class="admin-pill" v-if="user.isAdmin">admin</div>
    <div class="pill" v-else>not admin</div>
    <p style="font-weight:bold">Followers: {{followers}}</p>
    <button @click="followUser">Follow User</button>
    <form @submit.prevent="createNewTwoot"> <!-- prevent form reload and instead run function that adds a new item -->
        <textarea v-model="newTwootContent" rows="4" />
        <label>Type:</label>
        <select v-model="selectedTwootType">
          <option :value="option.value" v-for="(option, index) in twootTypes" :key="index">
            {{option.name}}
          </option>
        </select>
        <button>Twoot!</button>
    </form>
        {{newTwootContent}}
    <div style="list-style-type:none;padding-left: 0" v-for="twoot in user.twoots" :key="twoot.id">
      <TwootItem :twoot="twoot" :username="user.username" @favoriteTwoot="toggleFavorite" />
    </div>
  </div>
</template>

<script>
import TwootItem from './TwootItem'

export default {
  name: 'UserProfile',
  components: {
    TwootItem
  },
  data() { //data must be a method in Vue 3 - can't be an object
    return {
      newTwootContent: '',
      selectedTwootType: 'instant',
      twootTypes: [
        {value: 'draft', name: 'Draft'},
        {value: 'instant', name: 'Instant Twoot'}
      ],
      followers: 0,
      user: {
        id: 1,
        username: '_LeshayFace',
        firstName: 'Leshay',
        lastName: 'Adams',
        email: 'shay@gmail.com',
        isAdmin: false ,
        twoots: [
          {id: 1, content: "My first Twoot"},
          {id: 2, content: "Do you think the war is a joke?"},
        ]
      }
    }
  },
  //watches data point and runs function when it changes
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        alert(`${this.user.username} gained a follower`)
      }
    }
  },
  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}` //string literal
    }
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFavorite(id) {
      alert(`Favorited Twoot! ${id}`)
    },
    createNewTwoot() {
      if (this.newTwootContent && this.selectedTwootType !== 'draft') {
        this.user.twoots.unshift({ //unshift adds item to start of list and push adds item to end
          id: this.user.twoots.length + 1,
          content: this.newTwootContent,
        }) 
      }
      this.newTwootContent = '';
    }
  }
}
</script>

<style scoped>
  .container {
    padding: 24px;
    border: 1px grey solid;
    border-radius: 4px;
  }

  .admin-pill {
    background: rgb(197, 8, 8);
    border-radius: 4px;
    display: inline-block;
    padding: 2px 8px;
    color: white;
  }

  .pill {
    background: rgb(69, 0, 180);
    border-radius: 4px;
    display: inline-block;
    padding: 2px 8px;
    color: white;
  }
</style>
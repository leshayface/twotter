<template>
  <div>
    <p style="font-weight: bold">@{{username}}</p>
    <p>{{twoot.content}}</p>
    <h2 @click="favoriteTwoot(twoot.id)">♡</h2>
  </div>
</template>

<script>
export default {
  name: 'TwootItem',
  props: {
    username: {
      type: String,
      required: true,
    },
    twoot: {
      type: Object,
      required: true,
    }
  },
  methods: {
    favoriteTwoot(id) {
      this.$emit('favorite-twoot', id); //passing event/data to parent
    }
  }
}
</script>
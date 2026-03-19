<template>
  <section class="list section">
    <table class="table is-striped">
      <thead>
        <tr>
          <th>Year</th>
          <th>Prize</th>
          <th>Description</th>
          <th><abbr title="Not good">NG</abbr></th>
          <th><abbr title="Already done">Done</abbr></th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="winner in winners">
          <td>{{winner.year}}</td>
          <td>{{winner.prize}}</td>
          <td>{{winner.description}}</td>
          <td><span class="icon" v-if="winner.notgood"><i class="fa fa-exclamation"></i></span></td>
          <td><span class="icon" v-if="winner.done"><i class="fa fa-check"></i></span></td>
          <td>
            <a class="icon" v-on:click="editWinner(winner)"><i class="fa fa-edit"></i></a>
            <a class="icon" v-on:click="removeWinner(winner)"><i class="fa fa-trash"></i></a>
          </td>
        </tr>
      </tbody>
    </table>
  </section>
</template>

<script>
export default {
  name: 'list',
  props: ['winners'],
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  },
  created: function () {
    this.winners.sort(function(a, b) {
      return b.year < a.year
    })
  },
  methods: {
    removeWinner: function (winner) {
      this.$emit('remove', winner)
    },
    editWinner: function (winner) {
      this.$emit('edit', winner)
    }
  }
}
</script>

<template>
  <section class="add section">
    <div class="field">
      <label class="label">Year</label>
      <p class="control">
        <span class="select">
          <select v-model:value="year">
            <option v-for="year in years">{{year}}</option>
          </select>
        </span>
      </p>
    </div>
		<div class="field">
			<label class="label">Prize</label>
			<p class="control">
				<input class="input" type="text" placeholder="Prize" v-model:value="prize">
			</p>
		</div>
		<div class="field">
			<label class="label">Description</label>
			<p class="control">
				<textarea class="textarea" placeholder="Description" v-model:value="description"></textarea>
			</p>
		</div>
		<div class="field is-horizontal">
			<p class="control">
				<a class="button" :class="{'is-outlined': !done, 'is-primary': done}" @click="done=!done">
					<span class="icon"><i class="fa fa-check"></i></span>
					<span>Done</span>
				</a>
				&nbsp;
				<a class="button" :class="{'is-outlined': !notgood, 'is-danger': notgood}" @click="notgood=!notgood">
					<span class="icon"><i class="fa fa-exclamation"></i></span>
					<span>Not good</span>
				</a>
			</p>
		</div>
		<div class="field">
			<p class="control">
				<a class="button is-primary" @click="saveNew">Save entry</a>
			</p>
		</div>
  </section>
</template>

<script>
export default {
  name: 'add',
  data () {
    return {
        years: Array,
        year: '',
				description: '',
				prize: '',
				notgood: false,
				done: false,
				newWinner: {
					year: '',
					prize: '',
					description: '',
					notgood: false,
					done: false
				}
    }
  },
  created: function () {
    let years = []
    for (var i=1991; i<=2016; i++) {
      years.push(i)
    }
    this.years = years
  },
  methods: {
    saveNew: function () {
			this.newWinner.description = this.description
			this.newWinner.prize = this.prize
			this.newWinner.year = this.year
			this.newWinner.done = this.done
			this.newWinner.notgood = this.notgood
    	this.$emit('saveNew', this.newWinner)
			this.newWinner = {}
			this.description = ''
			this.prize = ''
			this.year = ''
			this.notgood = false
			this.done = false
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

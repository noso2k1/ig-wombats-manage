<template>
  <section class="edit section">
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
				<input class="input is-success" type="text" placeholder="Prize" v-model:value="prize">
			</p>
		</div>
		<div class="field">
			<label class="label">Description</label>
			<p class="control">
				<textarea class="textarea is-success" placeholder="Description" v-model:value="description"></textarea>
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
				<a class="button is-success" @click="saveEdited">Save entry</a>
				<a class="button is-success is-outlined" @click="cancel">Cancel</a>
			</p>
		</div>
  </section>
</template>

<script>
export default {
  name: 'edit',
	props: ['defaultWinner'],
  data () {
    return {
        years: Array,
        year: '',
				description: '',
				prize: '',
				notgood: false,
				done: false
    }
  },
  created: function () {
    let years = []
    for (var i=1991; i<=2016; i++) {
      years.push(i)
    }
    this.years = years

		this.description = this.defaultWinner.description
		this.prize = this.defaultWinner.prize
		this.year = this.defaultWinner.year
		this.notgood = this.defaultWinner.notgood
		this.done = this.defaultWinner.done
  },
  methods: {
    saveEdited: function () {
			let editedWinner = this.defaultWinner
			editedWinner.description = this.description
			editedWinner.prize = this.prize
			editedWinner.year = this.year
			editedWinner.notgood = this.notgood
			editedWinner.done = this.done

			this.description = ''
			this.prize = ''
			this.year = ''
			this.notgood = false
			this.done = false
			this.$emit('saveEdited', editedWinner)
    },
		cancel: function () {
			this.$emit('cancelEdited')
			this.description = ''
			this.prize = ''
			this.year = ''
			this.notgood = false
			this.done = false
		}
  },
	watch: {
		defaultWinner: function(editedWinner) {
			this.description = editedWinner.description
			this.prize = editedWinner.prize
			this.year = editedWinner.year
			this.notgood = editedWinner.notgood
			this.done = editedWinner.done
		}
	}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

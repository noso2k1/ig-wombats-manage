<template>
  <section class="bulk section">
    <div class="field">
      <label class="label">Paste ignoble entries</label>
      <p class="control"><textarea class="textarea is-info" v-model:value="pastedText"></textarea></p>
    </div>
		<p>
      <a class="button is-info is-outlined" @click="generateEntries">Generate entries</a>
      <a class="button is-info" @click="saveEntries">Save generated entries</a>
    </p>

    <div class="field">
      <label class="label">Year</label>
      <p class="control">
        <span class="select">
          <select v-model:value="yearSel">
            <option v-for="year in years">{{year}}</option>
          </select>
        </span>
      </p>
    </div>

    <table class="table">
      <thead>
        <tr>
          <th>Year</th>
          <th>Prize</th>
          <th>Description</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="winner in data">
          <td>{{winner.year}}</td>
          <td>{{winner.prize}}</td>
          <td>{{winner.description}}</td>
        </tr>
      </tbody>
    </table>

  </section>
</template>

<script>
import Compromise from 'compromise'

export default {
  name: 'bulk',
  data () {
    return {
      pastedText: '',
      data: Array,
      years: Array,
      yearSel: ''
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
    saveEntries: function () {
      this.$emit('saveBulk', this.data)
    },
    generateEntries: function() {
      if (this.pastedText) {
        let myWords = {
          'reference': 'Reference',
          'references': 'Reference',
          'published in': 'Reference',
          'who attended the ceremony': 'Note',
          'reproduction': 'Prize',
          'economics': 'Prize',
          'physics': 'Prize',
          'chemistry': 'Prize',
          'medicine': 'Prize',
          'psychology': 'Prize',
          'engineering': 'Prize',
          'peace': 'Prize',
          'biology': 'Prize',
          'literature': 'Prize',
          'perception': 'Prize',
          'management': 'Prize',
          'mathematics': 'Prize',
          'physiology and entomology': 'Prize',
          'neuroscience': 'Prize',
          'public health': 'Prize',
          'art': 'Prize',
          'arctic science': 'Prize',
          'nutrition': 'Prize',
          'safety engineering': 'Prize',
          'arcaeology': 'Prize',
          'archeology': 'Prize',
          'archaeology': 'Prize',
          'probability': 'Prize',
          'acoustic': 'Prize',
          'acoustics': 'Prize',
          'fluid dynamics': 'Prize',
          'meteorology': 'Prize',
          'communications': 'Prize',
          'entomology': 'Prize',
          'astronomy': 'Prize',
          'consumer engineering': 'Prize',
          'visionary technology': 'Prize',
          'biodiversity': 'Prize',
          'science education': 'Prize',
          'statistics': 'Prize',
          'managed health care': 'Prize',
          'environmental protection': 'Prize',
          'computer science': 'Prize',
          'astrophysics': 'Prize',
          'technology': 'Prize',
          'hygiene': 'Prize',
          'interdisciplinary research': 'Prize',
          'agricultural history': 'Prize',
          'ornithology': 'Prize',
          'aviation': 'Prize',
          'linguistics': 'Prize',
          'cognitive science': 'Prize',
          'veterinary': 'Prize',
          'transportation planning': 'Prize',
          'physiology': 'Prize',
          'public safety': 'Prize',
          'anatomy': 'Prize',
          'joint prize in biology and astronomy': 'Prize',
          'diagnostic medicine': 'Prize',
          'education': 'Prize'
        }

        let r = ''
        let data = []
        let winner = {}
        let sentences = []
        let txt = ''
        let t = nlp(this.pastedText, myWords)
        sentences = t.sentences().data()
        console.log(sentences)
        for (var i=0; i<sentences.length; i++){
          r = nlp(sentences[i].text, myWords)
          if (r.match('^#Prize').out('text') == '') {
            txt = txt + sentences[i].text
          } else {
            if (i !== 0) { 
              winner.description = txt
              winner.year = this.yearSel
              data.push(winner)
            }
            winner = {}
            winner.prize = r.match('^#Prize').get(0).out('normal')
            txt = sentences[i].text
          }
          txt = txt.replace(/\r?\n|\r/g, " ")
        }
        winner.description = txt
        winner.year = this.yearSel
        data.push(winner)
        this.data = data   // save the last one
      }
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>

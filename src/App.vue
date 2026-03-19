<template>
  <div id="app">
    <notify v-if="notify" @closeMess="notify=false" :msg="msgTxt"></notify>
    <nav class="nav">
      <div class="nav-left">
        <div class="nav-item">
          <a class="button is-warning" :href="exportJSON" download="backup_firebasenpm.json">Export data as JSON</a>
        </div>
      </div>
      <div class="nav-center">
        <div class="nav-item">
          <a class="button is-info" @click="viewBulk=!viewBulk" :class="{'is-outlined': viewBulk}">
            <span class="icon"><i class="fa fa-upload"></i></span>
            <span>Bulk save</span>
          </a>
          <a class="button is-primary" @click="viewAdd=!viewAdd" :class="{'is-outlined': viewAdd}">
            <span class="icon"><i class="fa fa-plus"></i></span>
            <span>Add new</span>
          </a>
        </div>
      </div>
    </nav>
    <bulk v-if="viewBulk" @saveBulk="saveBulk"></bulk>
    <add v-if="viewAdd" @saveNew="saveNew"></add>
    <edit v-if="viewEdit" @saveEdited="saveEdited" :defaultWinner="editedWinner" @cancelEdited="cancelEdited"></edit>
    <list v-if="winners.length > 0" :winners="winners" @remove="removeWinner" @edit="editWinner"></list>
  </div>
</template>

<script>
import List from './components/List'
import Bulk from './components/Bulk'
import Notify from './components/Notify'
import Add from './components/Add'
import Edit from './components/Edit'
import Firebase from 'firebase'

let config = {
  apiKey: 'AIzaSyASKXkoroHifKZoOBPr85TPOIRZaqOcdJo',
  authDomain: 'ig-wombat.firebaseapp.com',
  databaseURL: 'https://ig-wombat.firebaseio.com',
  projectId: 'ig-wombat',
  storageBucket: 'ig-wombat.appspot.com',
  messagingSenderId: '189612520088'
}

let app = Firebase.initializeApp(config)
let db = app.database()
let winnersRef = db.ref('winners')

export default {
  name: 'app',
  components: {
    List,
    Bulk,
    Notify,
    Add,
    Edit
  },
  firebase: {
    winners: winnersRef
  },
  data () {
    return {
      viewBulk: false,
      viewAdd: false,
      viewEdit: false,
      notify: false,
      msgTxt: '',
      editedWinner: {}
    }
  },
  computed: {
    exportJSON: function () {
      let data = "text/json;charset=utf-8," + encodeURIComponent(JSON.stringify(this.winners, null, "\t"));
      return "data:" + data
    }
  },
  methods: {
    saveBulk: function (data) {
      data.forEach(function(entry, index) {
        winnersRef.push(entry)
      })
      this.msgTxt = 'Entries saved into Firebase'
      this.notify = true
    },
    saveNew: function(newWinner) {
      winnersRef.push(newWinner)
      this.msgTxt = 'Entry saved into Firebase'
      this.notify = true
    },
    saveEdited: function(saveEdited) {
      this.viewEdit = false
      // console.log(saveEdited['.key'])
      winnersRef.child(saveEdited['.key']).child('description').set(saveEdited.description)
      winnersRef.child(saveEdited['.key']).child('prize').set(saveEdited.prize)
      winnersRef.child(saveEdited['.key']).child('year').set(saveEdited.year)
      winnersRef.child(saveEdited['.key']).child('notgood').set(saveEdited.notgood)
      winnersRef.child(saveEdited['.key']).child('done').set(saveEdited.done)
      this.msgTxt = 'Firebase entry updated'
      this.notify = true
    },
    cancelEdited: function () {
      this.viewEdit = false
    },
    removeWinner: function(winner) {
      console.log(winner['.key'])
    },
    editWinner: function(winner) {
      this.viewEdit = true
      this.editedWinner = winner
    }
  }
}
</script>

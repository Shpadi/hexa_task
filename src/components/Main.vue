<template>
  <div class="container">
    <add-note :length="length"></add-note>
    <add-note-textarea :length="length"></add-note-textarea>
    <div class="table">
      <draggable v-model="allNotes" :options="{group:'notes'}" @start="drag=true" @end="dragEnd()">
        <transition-group name="list-complete">
          <div class="list-complete-item" v-for="(note,index) in allNotes" :key="note.id">
            <show-note :note="note" :index="index" :maxIndex="allNotes.length"></show-note>
          </div>
        </transition-group>
      </draggable>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import AddNote from './Notes/addNote'
import ShowNote from './Notes/showNote'
import AddNoteTextarea from './Notes/addNoteTextarea'
import Draggable from 'vuedraggable'

Vue.component('draggable',Draggable)
Vue.component('show-note',ShowNote);
Vue.component('add-note',AddNote);
Vue.component('add-note-textarea',AddNoteTextarea)
export default {
  name: 'Main',
  data() {
      return {
          allNotes:[],
          length:0,

      }
  },
   mounted() {
       let checkLocal=JSON.parse(localStorage.getItem('notes'));
       if (checkLocal!=null){
           this.allNotes=JSON.parse(localStorage.getItem('notes'));
           this.length=this.allNotes.length;
       }

      this.$root.$on('addNoteTextarea',(event)=>{
          this.allNotes=this.allNotes.concat(event);
          this.length=this.length+event.length;
          localStorage.setItem('notes',JSON.stringify(this.allNotes));
      });
      this.$root.$on('editNoteTextarea',(event,id)=>{
          console.log(event);
          this.editValue(event,id);
      });
      this.$root.$on('addToArray',(event)=>{
          this.saveValue(event);
      });
      this.$root.$on('deleteNote',(event,id)=>{
          this.deleteValue(event,id);
      });
       this.$root.$on('saveNewNote',(event,id)=>{
           this.editValue(event,id);
       });
   },
   methods: {
      dragEnd() {
          localStorage.setItem('notes',JSON.stringify(this.allNotes));
      },
      saveValue(event) {
          this.allNotes.push(event);
          this.length++;
          localStorage.setItem('notes',JSON.stringify(this.allNotes));
      },
      deleteValue(event,id) {
          this.allNotes.splice(id,1);
          localStorage.setItem('notes',JSON.stringify(this.allNotes));
      },
      editValue(event,id) {
          this.allNotes[id]=event;
          localStorage.setItem('notes',JSON.stringify(this.allNotes));
          window.location.reload();
      }

   }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
.list-complete-item {
  padding: 4px;
  margin-top: 4px;
  transition: all 1s;
}

.list-complete-enter, .list-complete-leave-active {
  opacity: 0;
}
</style>

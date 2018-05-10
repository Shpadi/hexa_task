<template>
    <div >
        <div class="note-item">

            <div class="cell">
                <p v-if="!isEdit">{{name}}</p>
                <input class="input-data" v-else v-model="name" type="text">
            </div>
            <div class="cell">
                <p v-if="!isEdit">{{value}}</p>
                <input class="input-data" v-else v-model="value" type="text">
            </div>
            <div class="cell"  v-if="!isEdit" @click="startEdit()">
                <span>Редактировать</span>
            </div>
            <div class="cell" v-if="isEdit" @click="saveNote()">
                <span >Сохранить</span>
            </div>
            <div class="cell" v-if="isEdit" @click="editTextarea()">
                <span >Выгрузить в textarea</span>
            </div>
            <div class="cell" v-if="isEdit" @click="deleteNote()">
                <span >Удалить</span>
            </div>

        </div>

    </div>
</template>
<script>
    export  default  {
        props:['note','index','maxIndex'],
        data() {
          return {
              id:'',
              value:'',
              name:'',
              realId:this.note.id,
              isEdit:false,
          }
        },
        mounted() {
            this.id=this.note.id;
            this.name=this.note.name;
            this.value=this.note.value;
            this.$root.$on('editNoteTextarea',()=>{
                this.isEdit=false;
            })
        },
        methods: {
            startEdit() {
                this.isEdit=true;
            },
            deleteNote() {
                this.$root.$emit('deleteNote',this.note,this.index);
            },
            editTextarea() {
                this.$root.$emit('editNoteText',this.note,this.index);
            },
            saveNote() {

                let note={
                    id:this.id,
                    name:this.name,
                    value:this.value,
                };
                if (this.name && this.value) {
                    this.$root.$emit('saveNewNote',note,this.index);
                    this.isEdit=false;
                }
                else
                    alert('неправильно ведены данные');


            }
        }
    }
</script>
<style>
    .note-item {
        display: flex;
    }

    .cell{
        width: 150px;
        padding: 10px;
        text-align: center;
        border: 1px solid;
    }
    p {
        margin: 0;
    }
    .input-data {
        width: 100%;
    }
    .note-item {
        cursor: pointer;
    }
</style>
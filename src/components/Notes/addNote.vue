<template>
    <div class="add-note">
        <input v-model="name" type="text"/>
        <input v-model="value" type="text"/>
        <button @click="addNoteToArray">Добавить</button>
    </div>
</template>
<script>
    export  default  {
        props:['length'],
        data() {
            return {
                name:'',
                id:'',
                value:'',
                addItem:{},
                idVal:this.length,
            }
        },
        mounted() {
            let checkLocal=JSON.parse(localStorage.getItem('notes'));
            if (checkLocal!=null) {

            }
            this.$root.$on('addNoteTextarea',(event)=>{
                this.idVal=this.idVal+event.length;
            });
        },
        methods: {
            addNoteToArray() {
                if (this.name && this.value) {
                        this.sendNote();
                }
                else {
                    alert('Не все поля заполнены');
                }

            },
            sendNote() {

                this.addItem={
                    id:this.idVal++,
                    name:this.name,
                    value:this.value,
                };
                this.name='';
                this.value='';
                this.$root.$emit('addToArray',this.addItem);
            }
        }

    }
</script>
<style>
    .add-note {
        text-align: left;
        margin-bottom: 20px;
    }
    .add-note input {
        width: 148px;
    }
    .add-note button {
        width: 148px;
    }
</style>
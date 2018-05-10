<template>
    <div class="add-textatea">
        <p>Пример: 'name:value;' (точка с запятой в конце обязательна)  </p>
        <textarea v-model="valueText"></textarea><br>
        <button v-if="!isEdit" @click="addArray">Добавить</button>
        <button v-if="isEdit" @click="editNote">Сохранить</button>
    </div>
</template>
<script>
    export default  {
        props:['length'],
        data() {
          return {
              valueText:'',
              idVal:this.length,
              isEdit:false,
              idEdit:'',
          }
        },
        mounted() {
            let checkLocal=JSON.parse(localStorage.getItem('notes'));
            if (checkLocal!=null) {
                this.idVal=checkLocal[checkLocal.length-1].id;
            }
            this.$root.$on('addToArray',(event)=>{
                this.idVal++;

            });
            this.$root.$on('editNoteText',(event,id)=>{
                this.isEdit=true;
                this.editText(event,id);
            })
        },
        methods: {
            addArray() {
                if (this.valueText) {
                    let elements=this.valueText.split(';');
                    if (elements.length) {
                        elements.splice(elements.length-1,1);
                        let element_array=[];
                        elements.map((item)=>{
                            let item_arr=item.split(':');
                            if (item_arr.length!=2) {
                                alert('Неправильно введены данные')
                            }
                            else {

                                let new_element={
                                    id: this.idVal++,
                                    name: item_arr[0],
                                    value: item_arr[1],
                                }
                                element_array.push(new_element)
                            }
                        });
                        this.$root.$emit('addNoteTextarea',element_array);
                    }
                }
            },
            editText(event,id) {
                this.valueText=event.name+':'+event.value+';';
                this.idEdit=id;
            },
            editNote() {
                let elements = this.valueText.split(';');
                console.log(elements);
                if (elements.length == 2) {
                        let item_arr = elements[0].split(':');
                        if (item_arr.length != 2) {
                            alert('Неправильно введены данные')
                        }
                        else {

                            let new_element = {
                                id: this.idVal++,
                                name: item_arr[0],
                                value: item_arr[1],
                            }
                            this.$root.$emit('editNoteTextarea', new_element,this.idEdit);
                        }

                }else {
                    alert('Нельзя добавить элемент в редактировании')
                }
            }
        }
    }
</script>
<style>

.add-textatea {
    text-align: left;
}
.add-textatea textarea {
    width: 40%;
    height: 100px;
}
</style>
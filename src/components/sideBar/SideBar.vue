<script>
import axios from 'axios';
export default {
    data() {
        return {
            template:{title:"Title",description:'Description',content:[{type:'text',text:"",unChecker:false},]},
            addedNote:{},
            urlTest:'http://localhost:1010/note',
            urlMain:'https://back-todo-list-lovat.vercel.app/note'
            }
            },
    props:{
        notesList:Array,
    }       ,
    emits: ['deleteNote', 'setActive', 'addNote'],
    methods:{
        setActive(note){
            this.$emit('setActive',note)
        },
        async addNote(){
            await this.AddData()
            // this.$emit('addNote',this.addedNote)
        },
        deleteNote(id){
            this.$emit('deleteNote',id)
            axios.delete(`${this.urlMain}/${id}`)
        .then(response => {
          console.log('Data deleted successfully:', response.data);
        })
        .catch(error => {
          console.error('Error deleting data:', error);
        });
        },
        async AddData() {

            await axios.post(`${this.urlMain}`, this.template)
              .then(r=>r.data)
              .then(response => {
                console.log('Data updated successfully:', response);
                // this.addNote = response;
                this.$emit('addNote',response)
              })
              .catch(error => {
                console.error('Error updating data:', error);
              });
        }
    } 
};
</script>

<template>
    <div class="m">
        <h2>Notes 📝</h2>
        <button class="addbtn" @click="addNote">Add</button>
        <button>Ai</button>
    </div>
<!-- {{ notesList }} -->
<div class="main">
<div v-for="note in notesList" :key="note._id" class="note" @click="setActive(note)">
    <p >
        <strong :style="{textOverflow: 'ellipsis'}">{{ note.title }} </strong> <br>
        {{ note.description }}
    </p>
    <button class="trash" @click="deleteNote(note._id)">🗑️</button></div>


</div>
</template>

<style scoped>


.main{
    overflow-y:auto;
    height: 70%;
}
.main::-webkit-scrollbar-thumb{
    background-color: rgb(203, 122, 16);
}
.note{
    border:1px solid #ccc;
    border-radius: 10px;
    margin-top: 10px;
    padding-left: 10px;
    cursor: pointer;
    transition: .3s;
    display: flex;
    justify-content: space-between;
}
.note:hover{
    background-color: #2c2a2722;
    border:1px solid #e0e0e0;
}
button{
    border: none;
    outline: none;
    background-color: #2c2a271d;
    color: #303036;
    padding: 9px;
    border-radius: 5px;
}
button:hover{
    
    background-color: #3937342f;
    color: #303036;
}
.trash{
    
    border-radius: 10px;
}
.addbtn{
    margin-right: 10px;
}
</style>
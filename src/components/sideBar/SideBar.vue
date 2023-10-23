<script>
export default {
    data() {
        return {
            
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
        addNote(){
            this.$emit('addNote',{id:Date.now() ,title:"",description:'',content:[{type:'',text:""},]} )
        },
        deleteNote(id){
            this.$emit('deleteNote',id)
        }
    } 
};
</script>

<template>
<h2>Notes 📝</h2><button @click="addNote">Add</button>
<!-- {{ notesList }} -->
<div v-for="note in notesList" :key="note.id" class="note" >
    <p @click="setActive(note)">
        <strong>{{ note.title }} </strong> <br>
        {{ note.description }}
    </p>
    <button class="trash" @click="deleteNote(note.id)">🗑️</button>


</div>
</template>

<style scoped>
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
    background-color: #2c2a27;
    border:1px solid #e0e0e0;
}
button{
    border: none;
    outline: none;
    background-color: #2c2a27;
    color: #F0E3CA;
    padding: 9px;
    border-radius: 5px;
}
button:hover{
    
    background-color: #393734;
    color: #F0E3CA;
}
.trash{
    
    border-radius: 10px;
}
</style>
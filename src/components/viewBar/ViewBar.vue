<script>
import axios from 'axios';
export default {
    data(){
        return{
            showAddContent:false,
            urlTest:'http://localhost:1010/note',
            urlMain:'https://back-todo-list-lovat.vercel.app/note'
        }
    },
    props:{
        active:Object,
    } ,
    methods:{
        changeValue(){
            this.$emit('change',this.active)
            console.log(this.active.title)
            this.updateData()
        },
        deleteContent(id){
            this.active.content = this.active.content.filter(item => item.id !== id)
            this.updateData()
        },
        addContent(){
            this.active.content = [...this.active.content,{
            type:'checkbox',
            checked:false,
            text:"",
            unChecker:false,
            id:Date.now(),
          }]
            this.updateData()
        },
        changType(contentId){
            const content = this.active.content.find(item => item.id === contentId);

            if (content) {
                // Toggle content type
                content.type = content.type === 'text' ? 'checkbox' : 'text';

                // Emit change event to notify parent component of the update
                this.$emit('change', this.active);
                this.updateData()
            }
    },
    updateData() {

      axios.patch(`${this.urlTest}/${this.active._id}`, this.active)
        .then(response => {
          console.log('Data updated successfully:', response.data);
        })
        .catch(error => {
          console.error('Error updating data:', error);
        });
    }
        
    },
    emits: ['change'],
};
</script>

<template>
<h2>View 🪟</h2>
<!-- {{ active }} -->
<div class="" v-if="active._id != 0">
    <input placeholder="title" @blur="changeValue" class="texta text" type="text" v-model="active.title" />
    <input placeholder="description" @blur="changeValue" class="text" type="text" v-model="active.description" />
    <div class="main">
        <div v-for="content in active.content" class="box" :style="{
        marginTop:content.type === 'text'? '26px':'6px'}">
        <input @click="changType(content.id)" value="🔁" title="Change to checkbox or text" class="tr" type="button" name="" id="">

        <!-- {{ content }} -->
        <div class="content">
           <p v-if="content.type === 'text'" style="font-size: larger;"></p>
           <input @click="updateData" class="checkbox" v-model="content.checked" type="checkbox" name="done" v-else-if="content.type === 'checkbox'" id="">
           <input 
        placeholder="text" 
        @blur="changeValue" 
        class="text " 
        type="text" 
        :style="{ textDecoration: content.checked ? 'line-through' : 'none',
        fontSize: content.type === 'text' ? '1.1em' : '1em' ,
        }" 
            v-model="content.text" /> 
        </div>
        <div class="tr" title="Auto unCheck">⏰</div>
        <input @click="updateData" v-model="content.unChecker"  value="" title="Auto unCheck" class="tr" type="checkbox" name="" id="">

        <button class="trr" @click="deleteContent(content.id)">🗑️</button>
        </div>
    </div>
    <button @click="addContent" >➕</button>

    <div class="" v-show="showAddContent">
    bu
    </div>
</div>
<div v-else><br> ← Choose one note</div>
</template>

<style scoped>
.main{
    
    /* margin-top: 120px; */
    overflow-y:auto;
    height: 300px;
}

.box{
    margin-top: 10px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    border-radius: 5px;
    padding: 4px;
    padding-left: 9px;
    padding-right: 9px;
    transition: .3s;
    box-shadow: rgba(0, 0, 0, 0.05) 0px 0px 0px 1px, rgb(209, 213, 219) 0px 0px 0px 1px inset;
}
.box:hover{
    box-shadow: rgba(0, 0, 0, 0.05) 0px 0px 0px 1px, #ccd4e0 0px 0px 0px 1px inset;
}
    .content{
    /* border: 2px solid wheat; */
    margin-top: 0px;
    display: flex;
    gap: 10px;
    align-items: center;
    flex: 3;
    }
    .text{
    width: 100%;
    background-color: rgba(240, 248, 255, 0);
    border: none;
    outline: none;
    color: #303036;
    }
    .texta{
        font-size: large;
        /* width: 200%; */
    }
    button{
        margin-top: 20px;
    border: none;
    outline: none;
    background-color: #d6d2cb27;
    color: #303036;
    padding: 9px;
    border-radius: 5px;
    transition: .3s;
}
button:hover{
    
    background-color: #afa5951d;
    color: #F0E3CA;
}
.tr{
    cursor: pointer;
    font-size: .7em;
    padding: 0px;
    margin-top: 0px;
    border-radius: 3px;
    background-color: #d5ccbe10;
    border: none;outline: none;
}
.trr{
    cursor: pointer;
    font-size: .7em;
    padding: 0px;
    margin-top: 0px;
    margin-left: 20px;
    border-radius: 3px;
    background-color: #3e3e3e32;
    border: none;outline: none;
}
.tr:hover ~ .box {
    background-color: #ae7e36;
}
.check{
    margin-right: 5px;
}
.checkbox{
    margin-left: 20px;
}
</style>
<script>
export default {
    data(){
        return{
            showAddContent:false,
        }
    },
    props:{
        active:Object,
    } ,
    methods:{
        changeValue(){
            this.$emit('change',this.active)
        },
        deleteContent(id){
            this.active.content = this.active.content.filter(item => item.id !== id)
        },
        addContent(){
            this.active.content = [...this.active.content,{
            type:'checkbox',
            checked:false,
            text:"",
            id:Date.now(),
          }]
        },
        changType(contentId){
        const content = this.active.content.find(item => item.id === contentId);

        if (content) {
            // Toggle content type
            content.type = content.type === 'text' ? 'checkbox' : 'text';

            // Emit change event to notify parent component of the update
            this.$emit('change', this.active);
        }
    }
        
    },
    emits: ['change'],
};
</script>

<template>
<h2>View 🪟</h2>
<!-- {{ active }} -->
<div class="" v-if="active.id != 0">
    <input placeholder="title" @input="changeValue" class="texta text" type="text" v-model="active.title" />
    <input placeholder="description" @input="changeValue" class="text" type="text" v-model="active.description" />
    <div v-for="content in active.content" class="box">
        <!-- {{ content }} -->
        <div class="content">
    <p v-if="content.type === 'text'">⁕</p>
    <input v-model="content.checked" type="checkbox" name="done" v-else-if="content.type === 'checkbox'" id="">
    <input 
        placeholder="text" 
        @input="changeValue" 
        class="text " 
        type="text" 
        :style="{ textDecoration: content.checked ? 'line-through' : 'none',
        fontSize: content.type === 'text' ? '1.1em' : '1em' }" 
        v-model="content.text" /> 
    </div>

        <input @click="changType(content.id)" value="🔁" title="Change to checkbox or text" class="tr" type="button" name="" id="">
        <button class="tr" @click="deleteContent(content.id)">🗑️</button>
    </div>

    <button @click="addContent" >➕</button>

    <div class="" v-show="showAddContent">
    bu
    </div>
</div>
<div v-else><br> ← Choose one note</div>
</template>

<style scoped>
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
    box-shadow: rgba(0, 0, 0, 0.05) 0px 0px 0px 1px, #ae7e36 0px 0px 0px 1px inset;
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
    color: #F0E3CA;
    }
    .texta{
        font-size: large;
        /* width: 200%; */
    }
    button{
        margin-top: 20px;
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
.tr{
    cursor: pointer;
    font-size: .7em;
    padding: 0px;
    margin-top: 0px;
    border-radius: 3px;
}
.tr:hover ~ .box {
    background-color: #ae7e36;
}
.check{
    margin-right: 5px;
}
</style>
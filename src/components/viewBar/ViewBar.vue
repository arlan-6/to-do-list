<script>
export default {
    props:{
        active:Object,
    } ,
    methods:{
        changeValue(){
            this.$emit('change',this.active)
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
<div v-for="content in active.content">
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
fontSize: content.type === 'text' ? '1.1em' : 'auto' }" 
    v-model="content.text" /> </div>
</div>
<button>➕</button></div>
<div v-else><br> ← Choose one note</div>
</template>

<style scoped>
    .content{
        /* border: 2px solid wheat; */
        margin-top: 10px;
        display: flex;
        gap: 10px;
        align-items: center;
    }
    .text{
        width: 80%;
        background-color: rgba(240, 248, 255, 0);
        border: none;
        outline: none;
    color: #F0E3CA;
    }
    .texta{
        font-size: large;
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
</style>
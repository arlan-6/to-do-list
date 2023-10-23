<script >
import SideBar from './components/sideBar/SideBar.vue';
import ViewBar from './components/viewBar/ViewBar.vue';



export default {
  components:{
    SideBar,
    ViewBar,
  },
    data(){
    return{
      notes:[
        {id:1 ,title:"note one",description:'this is the content of note one',
        content:[
          {
            type:'text',
            text:"aaaaaaaaaaaaaaaaaaaaaaaa"
          },
          {
            type:'checkbox',
            checked:false,
            text:" xxxxxx   xxxxxxxxxxxx "
          },
          
          
        ]
      },
        {id:2 ,title:"note two",description:'this is the content of note two',
        content:[
          {
            type:'text',
            text:"aaaaaaaaaaaaaaaaaaaaaaaa"
          },
          {
            type:'checkbox',
            checked:false,
            text:" xxxxxx   xxxxxxxxxxxx "
          },
          
          
        ]},
        {id:3 ,title:"note three",description:'this is the content of note three',
        content:[
          {
            type:'text',
            text:"qqqqqqqqqq"
          },
          {
            type:'checkbox',
            checked:false,
            text:" qqqqq   xxxxxxxxxxxx "
          },
          
          
        ]}        
      ],
      active:{id:0}

      
    }
    
  },
  methods:{
    setactive(note){
      console.log("setActive")
      this.active = note;
    },
    change(change){
      this.notes = this.notes.filter((note)=>{
        return note.id != change.id
      })
      this.notes = [change,...this.notes,].sort((a, b) => b.id - a.id);

    },
    addNote(note){
      console.log("addNote")
      this.notes = [note,...this.notes,]
      this.active = note;
    },
    deleteNote(id){
      console.log('delete')
      this.notes = this.notes.filter(item => item.id !== id);
      if(this.active.id===id){
        this.active={id:0}
      }
    }
  }
}
</script>

<template>
  <div class="body">
  <h1>To-Do-List 📃</h1>
  <div class="container">
    <div class="containerSidebar">
      <SideBar :notesList="notes" @deleteNote="deleteNote" @setActive="setactive" @addNote="addNote" />
    </div>
    <div class="containerViewbar">
      <ViewBar @change="change"  :active="active"/>
    </div>
  </div>
    
  </div>
</template>

<style >
*{
  margin: 0;padding: 0;
}
  .body{
    margin: 0;
    padding: 0;
    background-color: #1B1A17;
    height: 100ch;
    /* width: .5vw; */
    /* border-radius: 30px; */
    padding: 50px;
    color: #F0E3CA;
  }
  .container{
    display: flex;
gap:20px;
    height: 70%;
  }
  .containerSidebar{
    background-color: #1F1E1B;
    border-radius: 20px;
    flex: 1;
    padding: 20px;
  }
  .containerViewbar{
    background-color: #1F1E1B;
    border-radius: 20px;
    flex: 3;
    padding: 20px;
  }
</style>

<script >
import SideBar from './components/sideBar/SideBar.vue';
import ViewBar from './components/viewBar/ViewBar.vue';

import axios from 'axios';


export default {


  components:{
    SideBar,
    ViewBar,
  },
    data(){
    return{
      notes:[  ],
      active:{_id:0},
      showViewBar:false,
      showSideBar:true,
      isMobile: false,
      url:'http://localhost:1010/note',
      url1:'https://back-todo-list-lovat.vercel.app/note',

    }
    
  },
  methods:{
    setactive(note){
      console.log("setActive")
      this.active = note;
    },
    change(change){
      this.notes = this.notes.filter((note)=>{
        return note._id != change._id
      })
      this.notes = [change,...this.notes,].sort((a, b) => b._id - a._id);

    },
    addNote(note){
      console.log("addNote")
      this.notes = [note,...this.notes,]
      this.active = note;
    },
    deleteNote(id){
      console.log('delete')
      this.notes = this.notes.filter(item => item._id !== id);
      if(this.active._id===id){
        console.log(id)
        console.log(this.active._id)
        this.active={_id:0}
      }
    },
    fetchData() {
      axios.get(this.url1)//00000000000000000000000000000000
        .then(response => {
          this.notes = response.data;
        })
        .catch(error => {
          console.error('Error fetching data:', error);
        });
    },
    toggleViewBar(){
      this.active={_id:0}
    },
    checkWidth() {
      this.isMobile = window.innerWidth < 800;
    }
  },
  created() {
    this.fetchData();
  },
  mounted() {
    this.checkWidth(); // Check initial width on mount
    window.addEventListener('resize', this.checkWidth); // Listen for window resize
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.checkWidth); // Remove event listener on component destroy
  },
}
</script>

<template>
  <div class="body">
  <h1>To-Do-List 📃<div class="back" @click="toggleViewBar" v-show="active._id!=0">←</div></h1>
  
  <div class="container" v-if="isMobile">
    <div class="containerSidebar" v-if="active._id==0">
      <SideBar :notesList="notes" @deleteNote="deleteNote" @setActive="setactive" @addNote="addNote" />
    </div>
    <div class="containerViewbar" v-else>
      <ViewBar @change="change"  :active="active"/>
    </div>
  </div>
  <div class="container" v-else>
    <div class="containerSidebar" >
      <SideBar :notesList="notes" @deleteNote="deleteNote" @setActive="setactive" @addNote="addNote" />
    </div>
    <div class="containerViewbar" >
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
    background-color: #F6AE2D;



    height: 50ch;
    /* width: .5vw; */
    /* border-radius: 30px; */
    padding: 50px;
    color: #303036;
  }
  h1{
    color: #9B2915;

  }
  .container{
    display: flex;
gap:20px;
    height: 90%;
  }
  .containerSidebar{
    border-radius: 20px;
    flex: 1;
    padding: 20px;
    background-color: #F0F7F4;
  }
  .containerViewbar{
    background-color: #F0F7F4;
    border-radius: 20px;
    flex: 3;
    padding: 20px;
  }





  @media (max-width: 575.98px) {
  /* styles for extra small devices */
  .back{
    background-color: #bb5644;
    padding-left: 3px;
    padding-right: 3px;
    border-radius: 5px;
    height: 60%;
    margin-left: 20px;
    display: flex;
    align-items: center;
  }
  .body{
    margin: 0;
    padding: 0;
    background-color: #F6AE2D;
    height: 50ch;
    padding: 50px;
    color: #303036;
  }
  h1{
    color: #9B2915;
    display: flex;
  }
  .container{
    display: flex;
gap:20px;
    height: 90%;
  }
  .containerSidebar{
    border-radius: 20px;
    flex: 1;
    padding: 20px;
    background-color: #F0F7F4;
  }
  .containerViewbar{
    background-color: #F0F7F4;
    border-radius: 20px;
    flex: 3;
    padding: 20px;
  }
}

@media (min-width: 576px) and (max-width:800px) {
  /* styles for small devices */
  .back{
    background-color: #bb5644;
    padding-left: 3px;
    padding-right: 3px;
    border-radius: 5px;
    height: 60%;
    margin-left: 20px;
    display: flex;
    align-items: center;
  }
  .body{
    margin: 0;
    padding: 0;
    background-color: #F6AE2D;
    height: 50ch;
    padding: 50px;
    color: #303036;
  }
  h1{
    color: #9B2915;
    display: flex;
  }
  .container{
    display: flex;
gap:20px;
    height: 90%;
  }
  .containerSidebar{
    border-radius: 20px;
    flex: 1;
    padding: 20px;
    background-color: #F0F7F4;
  }
  .containerViewbar{
    background-color: #F0F7F4;
    border-radius: 20px;
    flex: 3;
    padding: 20px;
  }
}

@media (min-width: 800px) and (max-width: 991.98px) {
  /* styles for medium devices */
  .back{
    display: none;
  }
}

@media (min-width: 992px) and (max-width: 1199.98px) {
  /* styles for large devices */
  .back{
    display: none;
  }
}

@media (min-width: 1200px) {
  /* styles for extra large devices */
  .back{
    display: none;
  }
}

</style>

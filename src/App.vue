<script setup>
  import {ref} from "vue";

  const showOverlay = ref(false);
  const newNote = ref("")
  const notes = ref([])
  const warningMessage=ref(false)

  function getRandomColor() {
    return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
  }

  const addNote = () => {
    if(newNote.value.length < 10){
      return warningMessage.value="Note needs to be 10 characters or more."
    }
    notes.value.push({
      id: Math.floor(Math.random()*1000000),
      text: newNote.value,
      date: new Date(),
      backgroundColor: getRandomColor() 
    })
    showOverlay.value = false;
    newNote.value = "";
    warningMessage.value = "";
  }
</script>
<template>
  <main>
    <div v-if="showOverlay" class="overlay">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
        <p v-if="warningMessage">{{warningMessage}}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showOverlay = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showOverlay = true">+</button>
      </header>
      <div class="cards-container">
        <div v-for="note in notes" :key="note.id" class="card" :style="{backgroundColor: note.backgroundColor}">
          <p class="main-text">{{note.text}}</p>
          <p class="date">{{note.date.toLocaleDateString("en-US")}}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
  main{
    height: 100vh;
    width: 100vw;
  }
  .container{
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
  }
  header{
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  h1{
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
  }
  header button{
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(21,20,20);
    border-radius: 100%;
    color: white;
    font-size: 20px;
  }
  .card{
    width: 225px;
    height: 225px;
    background-color: rgb(237,175,200);
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
    
  }
  .date{
    font-size: 12.5px;
    font-weight: bold;
    margin-left: 5px;
  }
  .cards-container{
    display: flex;
    flex-wrap: wrap;
  }
  .overlay{
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .modal{
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }
  .modal button{
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: rgb(52, 192, 99);
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
  }
  .modal .close{
    background-color: rgb(202, 69, 124);
    margin-top: 7px;
  }
  .modal p{
    color: crimson;
  }
</style>
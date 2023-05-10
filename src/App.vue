<script setup>
import { ref } from 'vue';

const showModal = ref(false)
const newNote = ref("")
const erroMessage = ref("")
const notes = ref([])

function getRandomColor(){
  return "hsl("+Math.random() * 360 + ", 100%, 75%)";
}
// notice the note.value is fro the state object above
const addNote = () =>{
  if (newNote.value.length < 10){
    return erroMessage.value = "note has to be more than ten characters"
  }
  notes.value.push({
    id: Math.floor(Math.random() * 1000),
    text:newNote.value,
    date: new Date(),
    backgroundColor: getRandomColor()
  })
  showModal.value = false
  newNote.value = ""
  erroMessage.value = ""
}

</script>
<template>
  <div v-if="showModal" class="overlay">
    <div class="modal">
      <textarea name="notes" v-model.trim="newNote" id="notes" cols="30" rows="10"></textarea>
      <p style="color: red;" v-if="erroMessage">{{ erroMessage }}</p>
      <button @click="addNote">Add Notes</button>
      <button class="close" @click="showModal = false">close</button>
    </div>
  </div>
  <div class="container">
    <header>
      <h1>Notes</h1>
      <button @click="showModal = true">Add note</button>
    </header>
    <div class="card-container">
      <div v-for="note in notes" :key="note.id" class="card" :style="{backgroundColor: note.backgroundColor }">
        <p class="main-text">
          {{ note.text }}
        </p>
        <p class="date">{{ note.date.toLocaleDateString("en-GB") }}</p>
        <p>{{ note.id }}</p>
      </div>
    </div>
  </div>
</template>
<style scoped>
.container{
  max-width: 90%;
  padding: 10px;
  margin: auto;
  background-color: aquamarine;
}
header{
  display: flex;
  justify-content: space-between;
  align-items: center;
}
h1{
  font-weight: bold;
  margin-bottom: 25px;
  font-size: 2rem;
}
header button{
  border: none;
  padding: 10px;
  widows: 10%;
  background-color: aqua;
  border-radius: 10px;
  color: #fff;
  font-size: 1.2rem;
  cursor: pointer;
}
.card{
  width: 30%;
  height: 30vh;
  padding: 10px;
  border-radius: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-bottom: 25px;
}
.date{
  font-size: .9rem;
  font-weight: 600;
}
.card-container{
  display: flex;
  width: 100%;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
}
.overlay{
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.7);
  z-index: 3;
  display: flex;
  justify-content: center;
  align-items: center;
}
.modal{
  width: 60%;
  background-color: #fff;
  border-radius: 10px;
  padding: 20px;
  position: relative;
  display: flex;
  flex-direction: column;
}
.modal button{
  padding: 10px;
  font-size: 1.4rem;
  background-color: blueviolet;
  border: none;
  color: #fff;
  cursor: pointer;
  margin-top: 15px;
}
.modal .close{
  margin-top: 5px;
  background-color: red;
}
</style>
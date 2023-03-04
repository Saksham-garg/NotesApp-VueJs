<script setup>
import { ref } from "vue"
const showModal = ref(false);
const newNote = ref("")
const noteContent = ref([])
const errorMessage = ref("")
function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}
const addNote = () => {
  if(newNote.value.length < 9){
    errorMessage.value = "Note should contain more than 10 characters";
    return;
  }
  noteContent.value.push({
    id : Math.floor(Math.random()*1000),
    text : newNote.value,
    backgroundColor : getRandomColor(),
    date : new Date()
  })
  showModal.value = false;
  newNote.value = "";
  errorMessage.value="";
}

</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea name="note" v-model.trim="newNote" id="note" cols="30" rows="10"></textarea>
        <p> {{ errorMessage }}</p>
        <button @click="addNote()">Add Note</button>
        <button id="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="card-container">
        <div v-for="note in noteContent" class="card" :key="note.id" :style="{backgroundColor:note.backgroundColor}">
          <p class="p-card">{{ note.text }}</p>
          <p class="p-date">{{note.date.toLocaleDateString("en-US")}}</p>
        </div>
      </div>
    </div>
  </main>
</template>

<style>
main {
  height: 100vh;
  width: 100vw;
  padding: 10px;
  color: black;
}
h1{
  font-size: 40px;
  font-weight: bold;
}
.container {
  max-width: 1000px;
  margin: 0 auto;
}

header {
  display: flex;
  justify-content: space-between;
  flex-direction: row;
  margin-bottom: 15px;
  align-items: center;
}

.card-container {
  display: flex;
  flex-wrap: wrap;
}

.card {
  width: 225px;
  height: 225px;
  padding: 10px;
  background-color: rosybrown;
  border-radius: 10px;
  margin-right: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-bottom: 10px;
}

.p-date {
  font-weight: bold;
}

header button {
  color: white;
  background-color: black;
  border-radius: 100%;
  height: 40px;
  width: 40px;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgb(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal {
  position: relative;
  display: flex;
  flex-direction: column;
  width: 750px;
  padding: 30px;
  border-radius: 10px;
  background-color: white;
}

.modal button {
  background-color: blueviolet;
  cursor: pointer;
  font-size: 15px;
  padding: 8px;
  margin-top: 7px;
  border: none;
}

.modal #close {
  background-color: red;
  cursor: pointer;
  margin-top: 7px;
  border: none;
}

.modal p{
  color: red;
}

</style>
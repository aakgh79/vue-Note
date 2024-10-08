
<template>
  <div v-if="showModal" class="overlay">
    <div class="modal">
      <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10"></textarea>
    <p v-if="errorMassage" ><strong>{{errorMassage}}</strong></p>
      <button id="close" @click="showModal=false ,newNote='',errorMassage='';">close</button>
      <button id="add" @click="addNote"> add note</button>
    
    </div>
  </div>
  <main>
    <div class="container">
      <header >
        <h1>Note</h1>
        <button class="adding" @click="showModal=true">+</button>
        <div class="wrapper">
        </div>
      </header>
      <div class="cards-container" >
        <div 
        v-for="note in Notes" class="card" 
        :style="{backgroundColor:note.backgroundcolor}"
        :key="note.id">
          <p class="main-text">{{ note.text }}</p>
          <p class="date">{{note.date.toLocaleDateString("en-US")}}</p>
        </div>
        </div>
      </div>
  </main>
</template>

<script setup>
import{ref,watch} from "vue";

const showModal =ref(false)
const newNote =ref("")
const errorMassage = ref("")
const Notes =ref([]);
function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}
const addNote = () => {
  if (newNote.value.length <= 8) {
      return errorMassage.value = "Note is too short." ; 
  }
    Notes.value.push({
      id: Math.floor(Math.random() * 120),
      text: newNote.value,
      date: new Date(),
      backgroundcolor: getRandomColor()
    });
    // showModal.value = false;
    newNote.value = "";
};
watch(newNote, async(newValue, oldValue) => {
    if (newValue.length > oldValue.length ) {
      errorMassage.value = '';
    }
  });

</script>

<style scoped>
main {
  max-width: 1000px;
  max-height: 1000px;
  margin: 0 auto;
  padding: 10px;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
}

.adding {

  height: 60px;
  width: 60px;
  cursor: pointer;
  padding: 10px;
  border: none;
  font-size: 20px;
  border-radius: 100%;
  background-color: black;
  color: white;
}

.card {
  width: 225px;
  height: 225px;
  background-color: rgb(237, 182, 44);
  border-radius: 15px;
  display: flex;
  justify-content: space-around;
  flex-direction: column;
  margin-bottom: 20px;
  margin-right: 25px;
  padding-left: 10px;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.date {
  font-size: 18px;
  font-weight: bold;
}


h1 {
  font-size: 6em;
  margin-left: 20px;
  color: rgb(165, 109, 218)
}

header {
  max-height: 100vh;
  align-items: center;
  display: flex;
  justify-content: space-between;
}

.modal {
  width: 750px;
  background-color: azure;
  border-radius: 10px;
  padding: 39px;
  position: relative;
  display: flex;
  flex-direction: column;
}

#add {
  padding: 10px 20px;
  font-size: 20px;
  width: 100%;
  background-color: blueviolet;
  border: none;
  color: beige;
  cursor: pointer;
  margin-top: 15px;
}

#close {
  background-color: brown;
  position: absolute;
  top: 5px;
  right: 39px;
  height:10%;
  font-size: 20px;
  border: none;
  color: beige;
  cursor: pointer;
}
.modal p{
color: red;
  margin-top: 8px;
  margin-bottom:  0;
  font-size: 18px;
}

/* @media (min-width: 1024px) {
  header {
    display: inline;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  } */


/* header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  } */

/* nav {
    text-align: left;
    margin-left: -1rem;
    font-size: 1rem;

    padding: 1rem 0;
    margin-top: 1rem;
  } */
</style>

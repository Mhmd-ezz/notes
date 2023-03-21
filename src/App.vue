
<script setup>
  import {ref} from "vue";

  const showModal = ref(false)
  const newNote = ref("")
  const notes = ref([])
  const errorMsg = ref("")

  const getRandomColor = () => {
    return "hsl(" + Math.random()*360 + ", 100%, 75%)"
  }

  const addNote = () => {
    if (newNote.value.length < 5 || newNote.value.length > 1000) {
      errorMsg.value = "Note must be more between 5 and 1000 characters"
      return
    }
    notes.value.push({
      id: Math.floor(Math.random()*1000),
      text: newNote.value,
      date: new Date(),
      backgroundColor: getRandomColor()
    });
    showModal.value = false
    newNote.value = ""
  }
</script>

<template>
  <main>

    <div class="overlay" v-show="showModal">
      <div class="modal">
        <textarea v-model.trim="newNote" name="note" id="note" cols="30" rows="10" minlength="5" maxlength="500" required></textarea>
        <small v-show="errorMsg">{{errorMsg}}</small>
        <button @click="addNote()">Add Note</button>
        <button class="close" @click="showModal=false">Close</button>
      </div>
    </div>
    <!-- //end overlay-->

    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal=true">+</button>
      </header>

      <div class="cards-container">

        <div v-for="note in notes"
             :key="note.id"
             class="card"
             :style="{backgroundColor: note.backgroundColor}"
        >
          <p class="main-text">
            {{note.text}}
          </p>
          <p class="date">
            {{note.date.toLocaleDateString()}}
          </p>
        </div>

      </div>
      <!-- //end cards-container-->
    </div>
    <!-- //end container-->
  </main>
</template>

<style scoped>
  main {
    height: 100vh;
    width: 100vw;
  }
  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0,0,0,0.77);
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .container {
    max-width: 1000px;
    padding: 10px;
    margin: 0 auto;
  }
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 75px;
  }
  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(999,999,999);
    border-radius: 100%;
    color: black;
    font-size: 20px;
  }
  .cards-container {
    display: flex;
    flex-wrap: wrap;
  }
  .card {
    width: 225px;
    height: 225px;
    background-color: rgb(237, 182, 44);
    padding: 10px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-right: 20px;
    margin-bottom: 20px;
  }
  .main-text {
    font-size: 16px;
    color: black;
  }
  .date {
    font-size: 13px;
    color: dimgrey;
  }

  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
  }
  .modal button {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
  }
  .modal .close {
    background-color: rgba(193, 15, 15);
    margin-top: 7px;
  }
  small {
    color: red;
  }

</style>

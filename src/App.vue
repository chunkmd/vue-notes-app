<script setup>
import { ref } from "vue";

const showModal = ref(false);
const newNote = ref("");
const errorMessage = ref("");
const notes = ref([]);

const data = JSON.parse(localStorage.getItem("notes"));
const padding = 20; // Add this line
if (data) {
  notes.value = data;
}

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%";
}

const addNote = () => {
  if (newNote.value.length < 2) {
    return (errorMessage.value = "Note needs to be 10 characters or more");
  }

  if (newNote.value.length > 170) {
    return (errorMessage.value = "Note needs to be less than 170 characters");
  }

  const today = new Date();
  const date = today.toLocaleDateString("ro-MD");

  notes.value.push({
    id: Math.floor(Math.random() * 2375853324),
    text: newNote.value,
    date: date,
    backgroundColor: getRandomColor(),
  });

  localStorage.setItem("notes", JSON.stringify(notes.value));

  showModal.value = false;
  newNote.value = "";
  errorMessage.value = "";
};

const removeNote = (noteId) => {
  const newNotes = notes.value.filter((note) => {
    return note.id !== noteId;
  });

  notes.value = newNotes;

  localStorage.setItem("notes", JSON.stringify(notes.value));
};
</script>

<template>
  <main>
    <div v-if="showModal" class="overlay">
      <div class="modal">
        <textarea
          v-model.trim="newNote"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNote">Add Note</button>
        <button class="close" @click="showModal = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModal = true">+</button>
      </header>
      <div class="cards-container">
        <div
          v-for="note in notes"
          :key="note.id"
          class="card"
          :style="{ backgroundColor: note.backgroundColor }"
        >
          <div class="noteText">
            <p class="main-text">{{ note.text }}</p>
          </div>
          <div class="noteFooter">
            <p class="date">
              {{ note.date }}
            </p>
            <button @click="removeNote(note.id)">X</button>
          </div>
        </div>
      </div>
    </div>
  </main>
</template>

<style scoped>
main {
  min-height: 100vh;
  height: auto;
  width: 100vw;
  background-color: rgb(255, 255, 255);
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
  color: black;
}

header button {
  border: none;
  padding: 10px;
  width: 50px;
  height: 50px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: 20px;
}

.card {
  width: 225px;
  height: auto;
  background-color: rgb(237, 182, 44);
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  margin-right: 20px;
  margin-bottom: 20px;
  color: rgb(21, 20, 20);
}

/* Media query for screens smaller than 600px */

.date {
  font-size: 12.5px;
  font-weight: bold;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}

.overlay {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(255, 232, 232, 0.77);
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  color: black;
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
  background-color: rgb(193, 15, 15);
  margin-top: 7px;
}

.modal p {
  color: rgb(193, 15, 15);
}

.noteFooter {
  display: flex;
  flex-direction: row;
  display: flex;
  align-items: space-between;
}

.noteFooter button {
  margin-left: auto;
  border: none;
  width: 28px;
  height: 28px;
  cursor: pointer;
  background-color: rgb(21, 20, 20);
  border-radius: 100%;
  color: white;
  font-size: auto;
}

@media only screen and (max-width: 600px) {
  main {
    height: auto;
    width: 100vw;
    background-color: rgb(255, 255, 255);
    overflow: scroll;
  }

  .body {
    height: auto;
    padding-bottom: 10%;
  }

  .container {
    padding-left: 10%;
    max-width: 600px;
    padding: 10px;
    margin: 0 auto;
    margin-left: 5%;
    margin-bottom: 10%;
  }

  .card {
    width: 100%;
    height: auto;
    min-height: 80%;
    background-color: rgb(237, 182, 44);
    padding: 20px;
    border-radius: 15px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    margin-bottom: 10px;
    color: rgb(21, 20, 20);

  }

  header button {
    border: none;
    padding: 10px;
    width: 50px;
    height: 50px;
    cursor: pointer;
    background-color: rgb(21, 20, 20);
    border-radius: 100%;
    color: white;
    font-size: 20px;
    margin-right: 6%;
  }
  h1 {
    font-weight: bold;
    margin-bottom: 25px;
    font-size: 40px;
    color: black;
  }

  .overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    max-height: 100vh;
    background-color: rgba(255, 232, 232, 0.77);
    z-index: 10;
    display: flex;
    align-items: top;
    justify-content: top;
    color: black;
  }

  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;

    position: relative;
    top: 10%;
    left: 50%;
    transform: translate(-50%, -50%);
    display: flex;
    flex-direction: column;
  }
}
</style>

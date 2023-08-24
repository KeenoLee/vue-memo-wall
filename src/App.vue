<script setup>
import { ref } from "vue";

const showModel = ref(false);
const newNote = ref("");
const notesList = ref([]);
const errorMessage = ref("");

function getRandomColor() {
  return "hsl(" + Math.random() * 360 + ", 100%, 75%)";
}

function uuid() {
  return ([1e7] + -1e3 + -4e3 + -8e3 + -1e11).replace(/[018]/g, (c) =>
    (
      c ^
      (crypto.getRandomValues(new Uint8Array(1))[0] & (15 >> (c / 4)))
    ).toString(16)
  );
}

const addNotesHandler = () => {
  if (newNote?.value?.length < 10) {
    return (errorMessage.value = "Notes must be at least 10 characters");
  }
  notesList.value.push({
    id: uuid(),
    text: newNote.value,
    date: new Date(),
    color: getRandomColor(),
  });
  showModel.value = false;
  newNote.value = "";
};
</script>

<template>
  <main>
    <div v-show="showModel" class="overlay">
      <div class="model">
        <textarea
          v-model="newNote"
          name="note"
          id="note"
          cols="30"
          rows="10"
        ></textarea>
        <p v-if="errorMessage">{{ errorMessage }}</p>
        <button @click="addNotesHandler">Add Note</button>
        <button class="close-btn" @click="showModel = false">Close</button>
      </div>
    </div>
    <div class="container">
      <header>
        <h1>Notes</h1>
        <button @click="showModel = true">+</button>
      </header>

      <div class="cards-container">
        <div class="card">
          <p class="main-text">
            {Template-Card} Lorem ipsum dolor sit amet consectetur, adipisicing
            elit. Nesciunt iure fugit harum modi beatae quidem eius possimus
            sapiente reprehenderit deleniti, adipisci corporis blanditiis velit
            recusandae ipsa tenetur dolore! Excepturi, explicabo.
          </p>
          <p class="date">{Template-Date}</p>
        </div>
        <div
          v-for="note in notesList"
          class="card"
          :key="note.id"
          :style="{ backgroundColor: note.color }"
        >
          <p class="main-text">
            {{ note.text }}
          </p>
          <p class="date">{{ note.date }}</p>
        </div>
      </div>
    </div>
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
  background-color: rgb(0, 0, 0, 0.48);
  display: flex;
  align-items: center;
  justify-content: center;
}

.model {
  width: 40rem;
  background-color: aliceblue;
  border-radius: 10px;
  padding: 30px;
  margin: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
}

.model button {
  padding: 10px 20px;
  font-size: 14px;
  width: 100%;
  background-color: rgb(116, 107, 197);
  color: white;
  border: 1px solid;
  border-radius: 8px;
  cursor: pointer;
  margin-top: 15px;
}

.model .close-btn {
  background-color: rgb(232, 100, 100);
}

.model p {
  color: red;
}
.container {
  max-width: 75rem;
  padding: 10px;
  margin: 0 20px;
}
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

h1 {
  font-weight: bold;
  font-size: 50px;
}

header button {
  border: none;
  padding: 6px;
  width: 30px;
  height: 30px;
  cursor: pointer;
  background-color: rgb(147, 240, 240);
  border-radius: 100%;
  color: rgb(1, 16, 29);
  font-size: 18px;
}

.cards-container {
  display: flex;
  flex-wrap: wrap;
}
.card {
  width: 16rem;
  height: 16rem;
  background-color: aquamarine;
  padding: 10px;
  border-radius: 15px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  margin: 0px 20px 20px 0px;
}

.date {
  font-size: 14px;
  font-size: bold;
}
</style>

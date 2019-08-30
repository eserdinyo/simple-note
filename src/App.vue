<template>
  <div id="app">
    <div class="container">
      <textarea
        @keyup.enter="save"
        v-model="message"
        autofocus
        placeholder="enter something"
        name
        id
        cols="30"
        rows="5"
      ></textarea>
      <button @click="save">Save</button>
    </div>
    <div v-if="notes.length > 0" class="notes-wrapper">
      <note :note="note" v-for="note in notes" :key="'note-'+note.id" />
    </div>
    <div v-else class="empty">
      <p>What a clean sheet!</p>
    </div>
  </div>
</template>

<script>
import note from "./components/Note";
import { EventBus } from "./main";

export default {
  name: "app",
  data() {
    return {
      message: "",
      notes: [],
      notes_key: "NOTES_KET"
    };
  },
  components: {
    note
  },
  methods: {
    save() {
      if (this.message) {
        let msg = {
          id: Math.random(),
          message: this.message
        };

        this.notes.push(msg);
        this.message = "";
      }
    }
  },
  created() {
    if (localStorage.getItem(this.notes_key)) {
      this.notes = JSON.parse(localStorage.getItem(this.notes_key));
    }

    //remove note
    EventBus.$on("remove", id => {
      this.notes = this.notes.filter(note => note.id != id);
    });
  },
  watch: {
    notes() {
      localStorage.setItem(this.notes_key, JSON.stringify(this.notes));
    }
  }
};
</script>

<style lang="scss">
body {
  background-color: #202528;
}

.container {
  width: 1200px;
  margin: 0 auto;
  text-align: center;
}

.notes-wrapper {
  width: 1200px;
  margin: 0 auto;
  margin-top: 50px;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  grid-gap: 20px;
}

textarea {
  background-color: transparent;
  border: 1px solid #646464;
  width: calc(100% - 60px);
  border-radius: 1px;
  padding: 20px 30px;
  color: #fff;
  outline: 0;
  font-size: 16px;
  resize: none;
  margin-top: 50px;
  font-family: "Chilanka", cursive;
  display: inline-block;
}

button {
  padding: 5px 20px;
  background-color: transparent;
  color: #fff;
  margin-top: 20px;
  border-radius: 1px;
  cursor: pointer;
  font-size: 14px;

  transition: all 0.2s;
  outline: 0;

  &:hover {
    border-color: #191919;
    background-color: #191919;
  }
}

.empty {
  text-align: center;
  font-size: 80px;
  color: #646464;
  font-family: "Chilanka", cursive;
  text-transform: uppercase;
}
</style>

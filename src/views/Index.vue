<template>
  <div class="index">
    <h1>{{ message }}</h1>
    <div v-for="puppy in puppies">
      <ul>
        <li>Id: {{ puppy.id }}</li>
        <li>Name: {{ puppy.name }}</li>
        <li>Breed: {{ puppy.breed }}</li>
        <button v-on:click="showPuppies(puppy)">More Info</button>
        <hr/>
      </ul>
    </div>
    <dialog id="puppy-details">
      <form method="dialog">
        <h2>Id: {{ currentPuppy.id }}</h2>
        <p>Name: <input type="text" v-model="currentPuppy.name"/></p>
        <p>Breed: <input type="text" v-model="currentPuppy.breed"/></p>
        <button>Close</button>
        <button v-on:click="updatePuppies(currentPuppy)">Update</button>
      </form>
    </dialog>
    Name:
    <input type="text" v-model="newPuppyParams.name" />
    <br>
    Breed:
    <input type="text" v-model="newPuppyParams.breed" />
    <br/>
    <button v-on:click="createPuppies()">New Puppy</button>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to Index.vue!",
      puppies: [],
      newPuppyParams: {},
      editPuppyParams: {},
      currentPuppy: {},
    };
  },
  created: function () {
    this.indexPuppies();
  },
  methods: {
    indexPuppies: function () {
      axios.get("/puppies").then((response) => {
        console.log(response.data);
        this.puppies = response.data;
      });
    },
    createPuppies: function () {
      axios.post("/puppies", this.newPuppyParams).then((response) => {
        console.log(response.data);
        this.puppies.push(response.data);
        this.newPuppyParams = {};
      });
    },
    showPuppies: function (puppy) {
      this.currentPuppy = puppy;
      console.log(this.currentPuppy);
      document.querySelector("#puppy-details").showModal();
    },
    updatePuppies: function (puppy) {
      console.log(puppy);
      console.log(this.currentPuppy);
      axios.patch(`/puppies/${this.currentPuppy.id}`, puppy).then((response) => {
        console.log(response.data);
      });
    },
  },
};
</script>

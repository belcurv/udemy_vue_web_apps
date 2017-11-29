<template>
  <div class="col-md-4" @click="switchCharacter">
    <div class="character-card">
      <div class="card-block">
        <h4 class="card-title">{{character.name}}</h4>
        <p class="card-text">Height: {{character.height}}cm</p>
        <p class="card-text">Mass: {{character.mass}}kg</p>
        <p class="card-text">Hair Color: {{character.hair_color}}</p>
        <p class="card-text">Eye Color: {{character.eye_color}}</p>
      </div>
    </div>
  </div>
</template>


<script>
  export default {

    props: ['id'],

    data() {
      return {
        character: {}
      };
    },

    methods: {
      fetchCharacter(id) {
        let apiUrl = `https://swapi.co/api/people/${id}`;
        let options = { method: 'GET' };

        fetch(apiUrl, options)
          .then( res => res.json())
          .then( json => this.character = json);
      },
      switchCharacter() {
        let randId = Math.floor(Math.random() * 83) + 1;
        this.fetchCharacter(randId);
      }
    },

    created() {
      this.fetchCharacter(this.id);
    }

  };

</script>

<style>
  .character-card {
    border: 2px solid #4fc08d;
    border-radius: 4px;
    cursor: pointer;
  }
</style>

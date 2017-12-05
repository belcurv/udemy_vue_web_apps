<template lang="html">
  <div class="col-md-4" @click="switchItem">
    <div class="item-card">
      <div class="card-block">
        <h4 class="card-title">{{item.name}}</h4>
        <div class="" v-for="(value, key, index) in item">
          <div v-if="index < 5">
            <strong>{{key}}</strong>: {{value}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    props   : ['passedItem', 'type'],
    data() {
      return {
        item: {}
      }
    },
    methods : {
      switchItem() {
        let apiUrl    = 'https://swapi.co/api';
        let options   = { method: 'GET' };
        let max       = this.type === 'vehicles' ? 39 : 63;
        let random_id = Math.floor(Math.random() * max) + 1;

        fetch(`${apiUrl}/${this.type}/${random_id}`, options)
          .then( res  => res.json() )
          .then( json => this.item =json );

      }
    },
    created() {
      this.item = this.passedItem;
    }
  }
</script>

<style lang="css">
  .item-card {
    border: 2px solid #4fc080;
    border-radius: 4px;
    padding: 5px;
    padding-bottom: 10px;
    cursor: pointer;
  }
</style>

<template lang="html">
  <div class="col-md-12">
    <Item
      v-for="(item, index) in items"
      key="index"
      :passed-item="item"
      :type="type"
    />
  </div>
</template>

<script>
  import Item from './Item.vue';
  export default {

    data() {
      return {
        // $route is made for us from Vue router
        type: this.$route.params.type,
        items: []
      }
    },

    watch: {
      '$route': 'fetchItems'
    },

    methods: {

      change() {
        this.type = this.$route.params.type
      },

      // populate an 'items' array with objects from the API
      fetchItems() {
        this.type   = this.$route.params.type;
        this.items  = [];  // start from clean slate
        let apiUrl  = 'https://swapi.co/api';
        let options = { method: 'GET' };
        let initial_ids = [1, 13, 14];

        for (let i in initial_ids) {
          let id = initial_ids[i];
          console.log(id);

          fetch(`${apiUrl}/${this.type}/${id}`, options)
            .then(res  => res.json())
            .then(json => this.items.push(json));
        }
      }
    },

    created() {
      this.fetchItems();
    },

    components: {
      Item
    }

  }
</script>

<style lang="css">
</style>

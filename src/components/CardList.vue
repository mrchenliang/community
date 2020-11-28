<template>
  <div class="card-list">
    <Card v-for = 'community in communities' :community = 'community' :key = 'community.id'/> 
  </div>
</template>

<script lang="ts">
import { Vue } from 'vue-property-decorator';
import Card from './Card.vue';

export default Vue.extend ({
  name: 'CardList',
  components: { Card },
  props: {
    communities: {
        type: Array,
        default: () => {
          return []
        },
    },
  },
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .card-list {
        width: 85vw;
        margin: 0 auto;
        display: grid;
        grid-template-columns: 1fr 1fr 1fr 1fr;
        grid-gap: 20px;
    }
    @media only screen and (max-width: 815px) {
        .card-list {
            width: 85vw;
            margin: 0 auto;
            display: grid;
            grid-template-columns: 1fr;
            grid-gap: 20px;
        }
    }
</style>

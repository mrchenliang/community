<template>
  <div class='card-container'>
    <div>
        <img class="center-cropped" alt='community-image' @error = 'setAltImage' :src= 'this.community.communityImgUrl'/>
    </div>
    <h2>{{this.community.communityName}}</h2>
    <h2>Units Available: {{this.community.counts}}</h2>
    <h2>Average Home Price: {{this.price}}</h2>
  </div>
</template>

<script lang="ts">
import { Vue } from 'vue-property-decorator';
import stock from '../assets/stock.jpeg';

export default Vue.extend ({
  name: 'Card',
  props: {
    community: {
      type: Object,
      default: () => {
        return {}
      },
    },
  },
  computed: {
    price(): string {
        return new Intl.NumberFormat('en-CA', { style: 'currency', currency: 'CAD' }).format(this.community.averagePrice)
    }
  },
  methods: {
    setAltImage(event: any): void {
        event.target.src = stock;
    }
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .card-container {
        display: flex;
        flex-direction: column;
        background-color: #42b983;
        border: 1px solid grey;
        border-radius: 5px;
        padding: 25px;
        -moz-osx-font-smoothing: grayscale;
        backface-visibility: hidden;
        transform: translateZ(0);
        transition: transform 0.25s ease-out;
    }

    .card-container:hover {
        transform: scale(1.05);
    }

    .center-cropped {
        width: 200px;
        height: 200px;
        background-position: center center;
        background-repeat: no-repeat;
        overflow: hidden;
        border-radius: 10px;
    }

        /* Set the image to fill its parent and make transparent */
    .center-cropped img {
        min-height: 100%;
        min-width: 100%;
        /* IE 8 */
        -ms-filter: "progid:DXImageTransform.Microsoft.Alpha(Opacity=0)";
        /* IE 5-7 */
        filter: alpha(opacity=0);
        /* modern browsers */
        opacity: 0;
    }
</style>

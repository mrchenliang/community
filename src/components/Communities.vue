<template>
  <div class = "communities">
    <div>
      <h1>Calgary Communities</h1>
      <div v-if = loading class = "loading">
        <LoadingBar/>
      </div>
      <h6 v-else>
        <SearchBar
          placeholder='Search Community'
          @change = 'updateSearch'
        />
        <CardList :communities = 'updatedCommunitesInfo'/>
      </h6>
    </div>
  </div>
</template>

<script lang="ts">
import { Vue } from 'vue-property-decorator';
import SearchBar from './SearchBar.vue';
import LoadingBar from './LoadingBar.vue';
import CardList from './CardList.vue';
import axios from 'axios'

export default Vue.extend ({
  name: 'Communities',
  components: { SearchBar, LoadingBar, CardList },
  data() {
    return {
      communities: [] as any,
      homes: [] as any,
      search: '',
    }
  },
  async created(): Promise<void> {
    try {
      let communitiesResponse = await axios({
        method: "get",
        url: 'https://a18fda49-215e-47d1-9dc6-c6136a04a33a.mock.pstmn.io/communities'
      })
      let homesResponse = await axios({
        method: "get",
        url: 'https://a18fda49-215e-47d1-9dc6-c6136a04a33a.mock.pstmn.io/homes'
      })
      this.homes = homesResponse.data;
      this.communities = communitiesResponse.data;
    } catch(err) {
      console.log(err)
    }
  },
  methods: {
    updateSearch(value: string): void {
      this.search = value;
    },
  },
  computed: {
    loading(): boolean {
      return this.communities.length < 1 || this.homes.length < 1
    },
    updatedCommunitesInfo(): any {
      let sums: any = {}, counts: any = {}, averages: any = [], communityId: string;
      for (let i = 0; i < this.homes.length; i++) {
        communityId = this.homes[i].communityId;
          if (!sums[communityId]) {
            sums[communityId] = 0;
            counts[communityId] = 0;
          }
          sums[communityId] += this.homes[i].price|| null;
          counts[communityId]++;
      }
      for (let i = 0; i < this.communities.length; i++) {
        communityId = this.communities[i].id;
          if (!sums[communityId]) {
            sums[communityId] = 0;
            counts[communityId] = 0;
          }
      }
      for(communityId in sums) {
        let communityName = this.communities.find((community: any) => community.id === communityId) && this.communities.find((community: any) => community.id === communityId).name || "(Unknown Community)";
        let communityImgUrl = this.communities.find((community: any) => community.id === communityId) && this.communities.find((community: any) => community.id === communityId).imgUrl || "../assets/stock.jpeg";
        averages.push({ communityId, averagePrice: (sums[communityId] / counts[communityId]) || 0, counts: counts[communityId], communityName, communityImgUrl });
      }
      averages.sort((a: any, b: any) => (a.communityName > b.communityName) ? 1 : ((b.communityName > a.communityName) ? -1 : 0)); 

      if(this.search) {
        const filterAverages = averages.filter((average: any) =>
          average.communityName.toLowerCase().includes(this.search.toLowerCase())
        );
        return filterAverages;
      } else {
        return averages; 
      }
    },
  },
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
a {
  color: #42b983;
}
</style>

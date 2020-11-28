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
        for (let i = 0; i < this.communities.length; i++) {
          communityId = this.communities[i].id;
            if (!(communityId in sums)) {
              sums[communityId] = 0;
              counts[communityId] = 0;
            }
            sums[communityId] += this.homes && this.homes[i].price|| null;
            counts[communityId]++;
        }

        for(communityId in sums) {
          let community: any = this.communities.filter((community: any) => {
            if (community.id === communityId) {
              return community
            }
          });
          averages.push({ communityId: communityId, averagePrice: sums[communityId] / counts[communityId], counts: counts[communityId], communityName: community[0].name, communityImgUrl: community[0].imgUrl  });
        }
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

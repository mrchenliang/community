<template>
  <input
    class="search"
    type="search"
    v-model="search"
    placeholder={placeholder}
    @keydown.tab = 'emitChange'
    @keydown.enter = 'emitChange'
    @blur = 'emitChange'
    @onfocus = 'emitChange'
    @change = 'emitChange'
    @input = "emitChange"
  />
</template>

<script lang="ts">
import { Vue } from 'vue-property-decorator';

export default Vue.extend ({
  name: 'SearchBar',
  methods: {
      emitChange(event: any): void {
          const newValue = event.target.value.trim()
          this.$emit ('change', newValue)
      }
  }
})
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    .search {
        outline: none;
        padding: 10px;
        width: 150px;
        line-height: 30px;
        margin-bottom: 30px;
    }
</style>

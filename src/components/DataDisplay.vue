<template>
  <div class="DataContent">
    <div 
      v-for="(item, index) in starWarsData?.slice($store.state.indexToRender.from, $store.state.indexToRender.to)" 
      :key="index"
    >
      <template v-if="item.category === 'people'">
        <DataCard 
          :id="item.id"  
          :marked="item.marked"   
          :name="item.item.name" 
          :type="item.category" 
          :birth-year="item.item.birth_year"
          :gender="item.item.gender"  
          :skin-colors="item.item.skin_color"  
        />
      </template>
      <template v-else-if="item.category === 'planets'">
        <DataCard 
          :id="item.id"   
          :marked="item.marked"  
          :name="item.item.name"  
          :type="item.category" 
          :terrain="item.item.terrain"  
          :population="item.item.population" 
          :diameter="item.item.diameter" 
        />
      </template>
      <template v-else-if="item.category === 'films'">
        <DataCard 
          :id="item.id"  
          :marked="item.marked" 
          :producer="item.item.producer" 
          :episode="item.item.episode_id" 
          :director="item.item.director" 
          :name="item.item.title" 
          :type="item.category" 
        />
      </template>   
      <template v-else-if="item.category === 'species'">
        <DataCard 
          :id="item.id" 
          :marked="item.marked"  
          :classification="item.item.classification" 
          :average-height="item.item.average_height" 
          :name="item.item.name" 
          :skin-colors="item.item.skin_colors" 
          :type="item.category"  
        />
      </template>
      <template v-else-if="item.category === 'vehicles'">
        <DataCard 
          :id="item.id" 
          :marked="item.marked"   
          :name="item.item.name"  
          :type="item.category"  
          :crew="item.item.crew" 
          :model="item.item.model" 
          :manufacturer="item.item.manufacturer"  
        />
      </template>
      <template v-else-if="item.category === 'starships'">
        <DataCard 
          :id="item.id" 
          :marked="item.marked"  
          :name="item.item.name" 
          :model="item.item.model"  
          :crew="item.item.crew" 
          :manufacturer="item.item.manufacturer" 
          :type="item.category"  
        />
      </template>
    </div>
  </div>
</template>

<script>
  import DataCard from "./DataCard.vue"
  import { mapGetters } from 'vuex';

  export default {
    components:{DataCard},
      computed: {
    ...mapGetters(['starWarsData']),
  },
  mounted() {
    this.$store.dispatch('loadStarWarsDataFromLocalStorage')
  }
    ,

  }
</script>

<style lang="scss">
  @mixin center-flex {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  @mixin flex-ships {
    display: flex;
    align-items: flex-start;
    justify-content: center;
    flex-wrap: wrap;
  }

    .DataContent {
      @include flex-ships();
      overflow-x: hidden;
      overflow-y: auto;
      width: 100%;
      padding:  0 100px;
    }
</style>
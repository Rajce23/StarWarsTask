<template>
  <div class="container">
    <div class="nav">
      <div class="bellowNavButtons">
        <button 
          class="btnBlack"
          @click="goBack" 
        >
          Go back
        </button>
      </div>
      <h1 
        v-if="item.name" 
        class="heading"
      >
        {{ item.name }}
      </h1>
      <h1 
        v-if="item.title" 
        class="heading"
      >
        {{ item.title }} 
      </h1>

      <div class="dataDisplay">
        <div class="simpleDataDisplay">
          <div 
            v-for="(data) in simpleData" 
            :key="data.name" 
            class="simple"
          >
            <h1>{{ data.name }}</h1>
            <p>{{ data.value }}</p>
          </div>
        </div>

        <div  
          v-for="(data, index) in complexData" 
          :key="index" 
          class="complexData"
        >
          <h1>{{ data.name }}</h1>

          <div>
            <div 
              v-for="(innerData,i) in data.values" 
              :key="i"
            > 
              <ComlexDataPeople 
                v-if="data.name.toLowerCase()==='pilots'|| data.name.toLowerCase()==='people' || data.name.toLowerCase()==='characters' || data.name.toLowerCase()==='residents' "
                :type="data.name"  
                :url="innerData"  
              />
              
              <ComplexDataFilms   
                v-if="data.name.toLowerCase()==='films'"
                :type="data.name"
                :url="innerData" 
              />
              
              <ComplexDataPlanets  
                v-if="data.name.toLowerCase()==='planets'"  
                :type="data.name"  
                :url="innerData"  
              />
              
              <ComplexDataSpecies   
                v-if="data.name.toLowerCase()==='species'"  
                :type="data.name"  
                :url="innerData"  
              />
              
              <ComplexDataStarShipsVehichles  
                v-if="data.name.toLowerCase()==='vehicles' || data.name.toLowerCase()==='starships'"
                :type="data.name"
                :url="innerData"   
              />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import ComlexDataPeople from "../components/ComplexDataCards/ComlexDataPeople.vue"
  import ComplexDataFilms from "../components/ComplexDataCards/ComplexDataFilms.vue"
  import ComplexDataPlanets from "../components/ComplexDataCards/ComplexDataPlanets.vue"
  import ComplexDataSpecies from "../components/ComplexDataCards/ComplexDataSpecies.vue"
  import ComplexDataStarShipsVehichles from "../components/ComplexDataCards/ComplexDataStarShipsVehichles.vue"
  import { mapGetters } from 'vuex';
  import { mapActions } from 'vuex';

  export default {
      components:{ComlexDataPeople,ComplexDataFilms,ComplexDataPlanets,ComplexDataSpecies,ComplexDataStarShipsVehichles},
    data(){
      return {
          item:{},
          simpleData:[],
          complexData:[],
          name:"",
      }
    },
      computed: {
      ...mapGetters(['itemToShow']),

    },
  watch: {
    itemToShow: function(newVal) {
      if(newVal.item.name !== undefined )
      {
      this.item  = newVal.item
      }
      else
      {
      this.item  = newVal.item
      }
      this.renderSimpleData()
      this.renderComplexData()
    }
  },
      mounted() {
      this.item = this.itemToShow.item
      this.id = this.itemToShow.id
      this.name = this.itemToShow.item.name
      this.renderSimpleData()
      this.renderComplexData()
    },

  methods:{
    ...mapActions(['setFavorite']),
    renderSimpleData(){

      const simpleData = []
      Object.values(this.item).map((data,index)=>{
        if (data && Array.isArray(data) === false && index !== 0 && data.length < 50 && data.includes("http") === false) {
          const str = Object.keys(this.item)[index];
          const newString = str.split('_').map(word => word.charAt(0).toUpperCase() + word.slice(1)).join(' ');
          if (newString === "Created" || newString === "Edited" ) {
            const date = new Date(data);
            simpleData.push({name:newString,value:date.toDateString()});
          } else {
            simpleData.push({name:newString,value:data});
          }
        }
      });
      this.simpleData = simpleData;
    },
      renderComplexData(){
      const complexData = []

      Object.values(this.item).map((data,index)=>
      {
        if(Array.isArray(data) && data.length >  0  )
        {
          const str = Object.keys(this.item)[index]
          const newString = str.split('_').map(word => word.charAt(0).toUpperCase() + word.slice(1)).join(' ');
          const dataToDisplay = []

          data.map((url)=>{
                    dataToDisplay.push(url)
              })
        complexData.push({name:newString,values:dataToDisplay})
        }
      })
        this.complexData = complexData
  },
      goBack() {
        this.$router.push('/')
      }
  },

  }
</script>

<style lang="scss">
  @font-face {
    font-family: 'Star Jedi';
    src: url('../../assets/Starjedi.ttf') format('truetype');
  }
    * {
      margin: 0;
      padding: 0;
      box-sizing: border-box;
      font-family: 'Star Jedi', sans-serif;
    }
  .nav
  {
    width: 100%;
    background-color: rgba(0, 0, 0, 0.87);
    height: 100px;
    padding: 30px;
  }
  .container {
    width: 100vw;
    height: 100vh;
    overflow-x: hidden;
  }
  .heading {
    text-align: center;
    font-size: 2.5rem;
  }
  .dataDisplay {
    width: 100%;
    overflow-x: hidden;
    overflow-y: auto;
    padding: 10px;

  }

  .simpleDataDisplay{
    width: 100%;
    margin: 40px 0;
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    }

  .bellowNavButtons {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-around;
    height: 50px;
    > button {
      width: 200px;
      height: 30px;
      border: none;
      color: white;
      font-size: 1.1rem;
      font-weight: 600;
      border-radius: 2px;
      cursor: pointer;
      transition: 0.4s;
      &:hover {
        transform: translateY(-5px);
        box-shadow: 0px 10px 20px 2px rgba(0, 0, 0, 0.25);
      }
    }
  }

  .btnBlack {
    background-color: rgba(0, 0, 0, 0.844);
  }
  .btnBlue {
    background-color: rgba(4, 80, 243, 0.719);
  }
  .simple {
    width: 300px;
    height: 200px;
    margin: 10px;
    border-radius: 5px;
    box-shadow: rgba(50, 50, 93, 0.25) 0px 2px 5px -1px,
    rgba(0, 0, 0, 0.3) 0px 1px 3px -1px;
    display: flex;
    align-items: center;
    justify-content: space-around;
    flex-direction: column;
    text-align: center;
    padding: 5px;
    letter-spacing: 4px;
    > h1 {
      font-size: 1.5rem;
    }
    > p {
      font-size: 1.25rem;
      text-align: center;
    }
  }

  .complexData{
  width: 100%;
  text-align: center;
  margin-bottom: 50px;
  >div
  {
  width: 100%;
        display: flex;
      justify-content: center;
      flex-wrap: wrap;
      padding-top: 40px;
  }
  }
</style>

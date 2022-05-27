<template>
    <div id="app">

        <app-header/>

        
            <app-modal :character = "Characters [characterIndex]"/>

            <spinner v-if="loading"/>

            <div class="row">
            
        <h1>Персонажи Marvel</h1>



        <div v-for="(el, idx) in Characters" :key="el.id" class="card mb-3" style="max-width: 540px;">
            <div class="row g-0">
              <div class="col-md-4">
                <img :src="el.thumbnail" class="img-fluid rounded-start" :alt="el.name">
              </div>
              <div class="col-md-8"> 
                <div class="card-body">
                  <h5 class="card-title">{{el.name}}</h5>
                  <p class="card-text"></p>
                  <!-- Button trigger modal -->
                  <button type="button" class="btn btn-secondary" data-bs-toggle="modal" data-bs-target="#exampleModal" @click="characterIndex = idx">
                    Подробнее
                  </button>
                </div>
              </div>
            </div>
          </div>

      </div>
  
            </div>
        
</template>

<script>
    import Spinner from "./components/Spinner";
    import AppModal from "./components/AppModal";
    import AppHeader from "./components/AppHeader";

    export default {
        name: 'App',
        components: {
            AppHeader,
            AppModal,
            Spinner,
        },
        data() {
            return {
                loading: false,
                characters: [],
                characterIndex: 0,
            }
        },
        methods: { 
            fetchCharacters: function (){
               return fetch ('https://netology-api-marvel.herokuapp.com/characters')
                      .then(res => res.json())
                      .then(json => this.characters = json)
            },
            changeSearch: function (value) {
              this.search = value 
            }
        },
        computed: {
          searchCharacters: function (){
           const {characters, search} = this
           return characters.filter (character => {
             return character.name.indexOf(search) !== -1;}
           )
          }
        },
       async mounted () {

          this.loading = true;
             await this.fetchCharacters();
            this.loading = false;
            
        }
    }
</script>

<style>

</style>

<template>
  <div id="app">
    <img src="./assets/logo.png">
    <img src="./assets/ES.png">
    <h1>{{ msg }}</h1>
  <div id='defaultForm'>
  </div>
  </div>
</template>



<script>

  import Vue from 'vue';
  import {Searchbox,
    SearchDatalist,
    RefinementListFilter,
    Paginate,
    SearchButton,
    ResetButton,
    Hits,Generics,NumericListFilter} from 'vue-innersearch/src/innerSearch';

      Vue.component('searchbox', Searchbox);
      Vue.component('search-datalist', SearchDatalist);
      Vue.component('refinement-list-filter', RefinementListFilter);
      Vue.component('paginate', Paginate);
      Vue.component('search-button', SearchButton);
      Vue.component('reset-button', ResetButton);
      Vue.component('hits', Hits);
      Vue.component('numeric-list-filter', NumericListFilter);

      Vue.mixin(Generics);

  window.addEventListener('load', function () {
    new Vue({
      el: '#defaultForm',

      created : function () {
        // ES server configuration
        this.setHost('http://198.74.56.26:9200');
        this.setIndex('tweet');
        this.setType('_doc');
       },


      template : `
                <section>
       <h1 class='is-title'>Search demo</h1><hr class='is-line' />
       <div> 
      <searchbox :autofocus="true" :realtime="true" :timeout="150" :field="'text'" :placeholder="'What can I help you with ?'"></searchbox>
      <hits>
        <template slot="hits" slot-scope="{ hits }">
           <div class="is-score is-hits">
            <strong v-if="hits.score === 0">No result found</strong>
            <strong v-else-if="hits.score === 1">1 result found</strong>
            <strong v-else-if="hits.score > 1">{{ hits.score }} results found</strong>
           </div>
                <div v-for="item in hits.items" :item="item">
                    <div>Found: {{ item._source.text }}</div>
                </div>
          </template>
        </hits>
                    </div>
                </section>
            `
    });
  });

export default {
  name: 'app',
  data () {
    return {
      msg: 'Welcome to yet another of lecw\'s features demo'
    }
  }
}
</script>

<style>
  @import url(https://unpkg.com/vue-innersearch@0.0.10/default-innersearch-theme.min.css)
</style>


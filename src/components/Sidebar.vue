<template>
  <div id="sidebar">
    <Clock/>
    <div class="addrss">
        <input class="rss-input" id="adder" type="text" v-model="newRss" v-on:keyup.enter="handleAdd" placeholder="Enter New RSS ...">
    </div>
    <RSSList v-bind:rsslist="rsslist" v-bind:changeSelected="changeSelected" v-bind:removeSelected="removeSelected" v-bind:refreshSelected="refreshSelected"  v-bind:isLoading="isLoading"/>
  </div>
</template>

<script>

// todo style list
// todo add removal links
// future python-side if url errors auto removal
// future refresh content automaticly/manually



import Clock from './Clock'
import RSSList from './RSSList'

export default {
  name: 'sidebar',
  props: ['rsslist', 'addRSS', 'changeSelected', 'removeSelected', 'refreshSelected', 'isLoading'],
  data: function(){
      return{
          newRss: ""
      }
  },
  components: {
    Clock,
    RSSList
  },
  methods: {
      handleAdd: function(){
          this.addRSS(this.newRss)
          this.newRss = ""
      }
  }
}
</script>

<style>
#sidebar {    
    flex-basis: 25%;
    background: #453a4c;
    box-shadow: 0 0 10px 3px #2d1c38;
    z-index: 2;
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    align-items: center;
}
.addrss{
    margin: 30px;
}
.rss-input{
    background-color: transparent;
    border: none;
    border-bottom: .5px solid aquamarine;
    padding: 5px;
    /* padding-left: 50px;
    padding-right: 50px; */
    width: 15rem;
    color: aquamarine;
    transition: .5s ease-in all;
}
.rss-input:focus{
    color: rgb(26, 224, 158);
    border-bottom: .5px solid rgb(26, 224, 158);
    transition: .5s ease-in all;
    animation-name: breath;
    animation-duration: 2s;
    animation-iteration-count: infinite;
}

@keyframes breath {
    0%, 100%{
        color: rgba(127, 255, 212, 0.555);
        border-bottom: .5px solid rgba(127, 255, 212, 0.555);
        transition: 1s ease-in all;
    }

    25%, 75%{
        color:rgba(127, 255, 212, 0.74);
        border-bottom: .5px solid rgba(127, 255, 212, 0.74);
        transition: 1s ease-in all;
    }

    50%{
        color:rgb(127, 255, 212);
        border-bottom: .5px solid rgb(127, 255, 212);
        transition: 1s ease-in all;
    }
}
</style>

<template>
  <div id="app">
    <Sidebar v-bind:rsslist="rsslist" v-bind:addRSS="addRSS" v-bind:changeSelected="changeSelected" v-bind:removeSelected="removeSelected" v-bind:refreshSelected="refreshSelected" v-bind:isLoading="isLoading"/>
    <Viewer v-bind:filteredList="filteredList"/>
    <Popup v-bind:errorPopup="errorPopup" v-bind:infoPopup="infoPopup" v-bind:isInfo="isInfo" v-bind:isError="isError"/>
  </div>
</template>

<script>
import axios from 'axios';

// future getOnly new Added

// future instead of sending all req once, loop req and load one by one !

// Todo cache or local storage

import Sidebar from './components/Sidebar';
import Viewer from './components/Viewer';
import Popup from './components/Popup';

// window.scroll({
// top:7500,
// left:0,
// behavior: 'smooth'
// })

export default {
  name: 'app',
  components: {
    Viewer,
    Sidebar,
    Popup
  },
  data: function(){
    return {
      rsslist: [],
      errorPopup: "",
      infoPopup: "",
      selectedTitle : "",
      isLoading : false,
      isInfo: false,
      isError: false
    }
  },
  created(){
    this.updater();
  },
  methods: {
    //* HandlePopups
    handleError: function(msg){
      this.errorPopup = msg;
      this.isError = true;
      setTimeout(() => {
        this.isError = false;
        this.errorPopup = "";
      },3000);
    },
    handleInfo: function(msg){
      this.infoPopup = msg;
      this.isInfo = true;
      setTimeout(() => {
        this.isInfo = false;
        this.infoPopup = "";
      },3000);
    },
    //* API Funcs
    updater: function(){
      //! Storage POWAHH!!!
      // future but add force update
      let storage = localStorage.getItem('fetchedRSS')
      if(storage !== null && JSON.parse(storage).length > 0){
        this.rsslist = JSON.parse(storage)
      }else{
        this.isLoading = true;
        axios.get("http://127.0.0.1:5000/get")
        .then(res => {
          if(res.data.success === true){
            this.rsslist = res.data.data;
            this.isLoading = false;
            localStorage.setItem('fetchedRSS', JSON.stringify(res.data.data));
            this.handleInfo("Fetched All RSS ...")
          }else{
          console.log(res.data.msg)
          this.handleError(res.data.msg)
          }
        })
        .catch(err => {
          console.log(err)
          this.handleError("Connection Error, Retry in 5 sec ...")
          setTimeout(() => {this.updater()}, 5000)
        })
        }
    },

    updateOnlyNew: function(url){
      axios.post("http://127.0.0.1:5000/getone", {URL: url})
      .then(res => {
        console.log(res.data.data)
        if(res.data.success === true){
          this.rsslist.push(res.data.data);
          localStorage.setItem('fetchedRSS', JSON.stringify(this.rsslist));
        }else{
          console.log(res.data.msg)
          this.handleError(res.data.msg)
        }
      })
      .catch(err => {
        console.log(err)
        this.handleError("Failed to Get RSS Info ...")
      })
    },

    addRSS: function(url){
      //! CORS crossDomain: true
      axios.post("http://127.0.0.1:5000/add", {URL: url}) 
      .then(res => {
        if (res.data.success === true) {
          this.updateOnlyNew(url)
          this.handleInfo("RSS Added and Fetched ...")
        }else{
          this.handleError(res.data.msg)
        }
      })
      .catch(err => {
        console.log(err)
        this.handleError("Failed to add RSS URL ...")
      })
    },
    refreshRSS: function(id){
      axios.post("http://127.0.0.1:5000/refresh", {CID: id})
      .then(res => {
        console.log(res.data)
        if(res.data.success === true){
          var rss = res.data.data
          this.rsslist.map(r => {
            if(r.cid === id){
              r = rss;
            }
          })
          localStorage.setItem('fetchedRSS', JSON.stringify(this.rsslist));
          this.handleInfo("RSS Refreshed ...")
        }else{
          console.log(res.data.msg)
          this.handleError(res.data.msg)
        }
      })
      .catch(err => {
        console.log(err)
        this.handleError("Failed to Get RSS Info ...")
      })
    },
    removeRSS: function(id){
      axios.delete("http://127.0.0.1:5000/remove", { data: {CID: id}})
      .then(res => {
        console.log(res.data)
        if(res.data.success === true){
          this.handleInfo(res.data.msg)
        }else{
          this.handleError(res.data.msg)
        }
      })
      .catch(err => {
        console.log(err)
        this.handleError("Failed to Get RSS Info ...")
      })
    },
    //* Other Funcs
    changeSelected(id){
      this.selectedTitle = id
    },
    removeSelected(id){
      //! remove from list here and from server
      //* improve confirmation popup
      var result = window.confirm("Do you really want to delete?");
      if (result) { 
        this.removeRSS(id);
        this.rsslist = this.rsslist.filter(r => {
          if(r.cid !== id){
            return r
          }
        })
        if (JSON.parse(localStorage.getItem('fetchedRSS')).length < 1){
          localStorage.removeItem('fetchedRSS')
        }
        //! database side
        localStorage.setItem('fetchedRSS', JSON.stringify(this.rsslist));
      }  
    },
    refreshSelected(id){
      this.refreshRSS(id);  
    }
  },
  computed: {
    filteredList: function(){
      if (this.selectedTitle === ""){
        return null;
      } else{
        var f = this.rsslist.filter(item => {
          return item.cid === this.selectedTitle;
        })
        return f[0];
      }
    }
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css?family=Ubuntu');
*{
  padding: 0;
  margin: 0;
  font-family:  'Ubuntu', sans-serif;
  line-height: 1.35rem;
}

body, html{
  background-color: #323232;
  height: 100vh;
  width: 100vw;
  overflow-x: hidden;
  overflow-x: hidden;
}

#app {
  height: 100vh;
  width: 100vw;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  display: flex;
  flex-direction: row;

}
/* highlight custom color */
::selection {
  background: rgba(29, 179, 129, 0.534);
}
::-moz-selection {
  background: rgba(29, 179, 129, 0.534);
}
/* Scroll-Bar */
body::-webkit-scrollbar {
    width: 1em;
}
 
body::-webkit-scrollbar-track {
    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);
}
 
body::-webkit-scrollbar-thumb {
  background-color: darkgrey;
  outline: 1px solid slategrey;
}
</style>

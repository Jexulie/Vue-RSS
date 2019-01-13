<template>
  <div id="rss-list">
    <div class="loading" v-if="isLoading">
        Refreshing ...
        <!-- animation or something -->
    </div>
    <div v-else-if="rsslist.length === 0" class="empty">
        Enter New RSS URL to Start ...
    </div>
    <ul v-else>
        <li v-for="item in rsslist" :key="item.cid">
            <div class="node">
                <i v-on:click="removeSelected(item.cid)" class="material-icons delete">delete</i>
                <a v-on:click="changeSelected(item.cid)">{{ item.title }}</a>
                <i v-on:click="refreshSelected(item.cid,)" class="material-icons refresh">refresh</i>
            </div>
        </li>
    </ul>
  </div>
</template>

<script>
/* Todo Loader */
export default {
  name: 'rss-list',
  props: ['rsslist', 'changeSelected', 'removeSelected', 'refreshSelected', 'isLoading'],
  components: {
    
  }
}
</script>

<style scoped>
#rss-list {
    margin: 25px;
    width: 80%;
    flex-basis: 100%;
    background: #29202e;
    box-shadow: 0 0 15px 1px #4fb8a1;
    border-radius: 20px;
    z-index: 2;
    display: flex;
}
.empty, .loading{
    width: 100%;
    margin: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    overflow-x: hidden;
    overflow-y: auto;
    height: 500px;
    color: #fff;
}
ul {
    width: 100%;
    margin: 20px;
    list-style: none;
    display: flex;
    flex-direction: column;
    align-items: center;
    overflow-x: hidden;
    overflow-y: auto;
}
li{
    margin: 5px;
    width: 100%;
    text-align: center;
    transition: .5s ease-in all;
    border-radius: 25px;
}
.node{
    padding: 8px;
    width: 100%;
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}
.refresh{
    margin-right: 10px;
    color:#fff;
    cursor: pointer;
    transition: .5s ease-in all;
}
.refresh:hover{    
    color: rgb(102, 212, 255);
    transition: .5s ease-in all;
    transform: rotate(360deg);
}
.delete{
    color: rgb(192, 59, 103);
    cursor: pointer;
    transition: .5s ease-in all;
}
.delete:hover{
    color: rgb(247, 97, 130);
    transition: .5s ease-in all;
}
a{
    color:aquamarine;
    cursor: pointer;
}
a:hover{
    /* color:rgb(35, 153, 114); */
    text-decoration: none;
    color: rgba(127, 255, 212, 0.555);
    transition: .5s ease-in all;
}
a:active{
    color: #444;
}
</style>

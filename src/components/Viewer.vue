<template>
  <div id="viewer">
    <div  v-if="filteredList !== null" class="content">
      <div class="info">
        <a :href="filteredList.link" target="_blank"><span class="channel-title">{{ filteredList.title }}</span></a>
        <span class="channel-desc" v-html="filteredList.description"></span>
      </div>
      <div class="list">
        <ul>
          <li v-for="item in filteredList.itemList" :key="item.id" class="li">
            <div class="nodes">
              <a :href="item.guid" target="_blank"><span class="title">{{ item.title }}</span></a>
              <span class="pubDate">{{ item.pubDate }}</span>
              <div class="desc" v-html="item.description"></div>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <div class="empty" v-else>
      <span class="empty-text">Select A RSS from List ...</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'viewer',
  props: ['filteredList'],
  components: {
    
  }
}
</script>

<style>
#viewer {
  z-index: 1;
  flex-basis: 75%;
  /* background-color: #685a71; */
  background: linear-gradient(75deg,#ff8a8a, #b887e6); /* Meh */
  height: 100vh;
  max-width: 100vw;
  overflow-x: hidden;
  overflow-y: auto;
  display: flex;
}

/* Content */
.content{
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* Empty */
.empty{
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  width: 100vw;
}
.empty-text{
  font-size: 1.5em;
  color:aquamarine;
}

/* Info */
.info{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  margin-top: 50px;
  margin-bottom: 50px;
  padding: 75px;
  width: 65%;
  background: #7e6b8a;
  box-shadow: 0 0 8px 1px #15071f;

}
.channel-title{
  font-size: 1.5em;
  transition: .5s ease-in all;
}
.channel-title:hover{
  color: rgb(85, 106, 224);
  transition: .5s ease-in all;
}
.channel-desc{
  margin-top: 20px;
  font-size: 1.25em;
  color: rgb(160, 173, 245);
}

/* List */
.list{
  display: flex;
  flex-direction: row;
  margin-left: 50px;
  margin-right: 50px;
}
.li{
  list-style: none;
}
.nodes{
  display: flex;
  flex-direction: column;
  margin: 30px;
  padding: 15px;
  box-shadow: 0 0 8px 1px #15071f;
  background-color: #685a71;
}
.title{
  font-size: 1.5em;
}
.pubDate{
  color: #f9d17c;
}
.desc{
  display: flex;
  flex-direction: column; /* Donno */
  color: #fff;
}
.desc > * {
  padding: 5px;
}
a{
  text-decoration: none;
  color: aquamarine;
  transition: .5s ease-in all;
}
a:hover{
  color: rgb(85, 106, 224);
  transition: .5s ease-in all;
}
img{
  width: 500px;
  height: auto;
  align-self: center;
}
</style>

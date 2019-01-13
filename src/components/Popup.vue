<template>
  <div id="popup">
    <div v-bind:class="[isInfo ? active : hidden, info]">
        <span>{{ infoPopup }}</span>
    </div>
    <div v-bind:class="[isError ? active: hidden, error]">
        <span>{{ errorPopup }}</span>
    </div>
  </div>
</template>

<script>
/* Todo Loader */
export default {
  name: 'popup',
  props: ['errorPopup', 'infoPopup', 'isInfo', 'isError'],
  data: function(){
    return {
        active: "active",
        hidden: "hidden",
        info: "pop pop-window-info",
        error: "pop pop-window-error"
    }
  },
  methods: {

  }
}
</script>

<style scoped>
#popup{
    position: absolute;
    top: 0;
    z-index: 50;
}
.pop {
    display: flex;
    justify-content: center;
    align-items: center;
}
.pop-window-error{
    position: absolute;
    top: 0;
    left: 70vw;
    width: 375px;
    height: 50px;
    margin-top: 50px;
    z-index: 50;
    background-color: rgb(255, 128, 128);
    box-shadow: 0 0 5px 3px rgb(177, 90, 90);
    border-radius: 10px;
    color: #fff;
    font-size: 1.25em;
    transition: .5s ease-in all;
}
.show{
    transition: .5s ease-in all;
    opacity: 1;
}
.hidden {
    transition: .5s ease-in all;
    opacity: 0;
}

.pop-window-info{
    position: absolute;
    top: 0;
    left: 30vw;
    width: 375px;
    height: 50px;
    margin-top: 50px;
    z-index: 50;
    display: flex;
    justify-content: center;
    align-items: center;
    background-color: rgb(153, 255, 128);
    box-shadow: 0 0 5px 3px rgb(107, 177, 90);
    border-radius: 10px;
    color: rgb(10, 8, 8);
    font-size: 1.25em;
    transition: .5s ease-in all;
}
</style>

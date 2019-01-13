<template>
  <div id="clock">
    <span class="hour digits">{{ this.time.getHours().toString().padStart(2, '0') }}</span><span class="sep">.</span>
    <span class="min digits">{{ this.time.getMinutes().toString().padStart(2, '0') }}</span><span class="sep">.</span>
    <span class="sec digits">{{ this.time.getSeconds().toString().padStart(2, '0') }}</span>
  </div>
</template>

<script>

export default {
  name: 'clock',
  components: {
    
  },
  created: function(){
      this.timeGetter = setInterval(this.getTime, 1000);
  },
  beforeDestroy: function(){
      clearInterval(this.timeGetter)
  },
  data: function(){
      return {
          time : new Date()
      }
  },
  methods: {
      getTime: function(){
          this.time = new Date();
      }
  },
  computed: {
      
  }
}
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css?family=Libre+Barcode+128+Text');
#clock {    
   margin-top: 30px;
   background-color: rgba(45, 25, 65, 0.315);
   padding: 20px;
   border-radius: 20px;
   box-shadow: 0 0 8px 1px #2d1c38;
   transition: 1s ease-in all;
   animation-name: breath;
   animation-duration: 2s;
   animation-iteration-count: infinite;
}
.digits {
    font-size: 3em;
    font-family: 'Libre Barcode 128 Text', cursive;
    color:rgba(127, 255, 212, 0.555);
    border-bottom: .5px rgba(127, 255, 212, 0.493) solid;
}

.sep{
    padding: 5px;
    font-family: 'Libre Barcode 128 Text', cursive;
    color:rgba(127, 255, 212, 0.493)
}

@keyframes breath {
    0%, 100%{
        box-shadow: 0 0 8px 1px rgba(127, 255, 212, 0.555);
        transition: 1s ease-in all;
    }

    25%, 75%{
        box-shadow: 0 0 8px 1px rgba(127, 255, 212, 0.74);
        transition: 1s ease-in all;
    }

    50%{
        box-shadow: 0 0 8px 1px rgb(127, 255, 212);
        transition: 1s ease-in all;
    }
}
</style>

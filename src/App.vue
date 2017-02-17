<template>
  <div id="app">

    <div class="ui secondary pointing menu">
      <img id="logo" src="/static/logo.png">
      <div class="right menu">
        <a id="menu_item" class="active lightBlue item" @click="routing = 'HomePage'">
          Home
        </a>
        <a id="menu_item" class="item" @click="routing = 'Demo1'">
          Demo1
        </a>
        <a id="menu_item" class="item" @click="routing = 'Demo2'">
          Demo2
        </a>
      </div>
    </div>

    <div id="contents">
    <HomePage v-if="routing == 'HomePage'"></HomePage>
    <Demo1 v-if="routing == 'Demo1'"></Demo1>
    <Demo2 v-if="routing == 'Demo2'"></Demo2>
    </div>
    

</div>
</template>

<script>
  /* eslint-disable */
  import HomePage from './components/HomePage'
  import Demo1 from './components/Demo1'
  import Demo2 from './components/Demo2'

  var Card = function(image, title, date, description){
    this.id = this.cid++;
    this.image = image || '/static/default.png';
    this.title = title;
    this.date = date;
    this.description = description;
  }

  export default {
    name: 'app',
    components:{
      Demo1,
      Demo2,
      HomePage
    },
    data () {
      return {
        routing: "HomePage"
      }
    },
    methods:{
    },
    mounted: function(){

      $(document).ready(function(){
        $('.ui.secondary.pointing.menu a.item').on('click', function(){
          $(this).addClass('active lightBlue').siblings().removeClass('active lightBlue');

        });
      });
    }
  }
</script>

<style>
  #app {
    font-family: 'Roboto';
    text-align: center;
    color: #2c3e50;
  }

  #logo{
    height: 30px;
    margin-top: auto;
    margin-bottom: auto;
  }


  .ui.secondary.pointing.menu{
    font-size: 18px;
    padding-left: 50px;
    padding-right: 50px;
    height: 25px;
    background-color: #575b5b;
  }
  .ui.secondary.pointing.menu .active.lightBlue.item{
    color: #00b4d4;
    border-color: #00b4d4;
    border-bottom-width: 4px;


  }

  .ui.secondary.pointing.menu .active.lightBlue.item:hover{
    color: #00b4d4;
    border-color: #00b4d4;
    

  }

  .ui.secondary.pointing.menu .item{
    color: #ffffff;
  }

  #menu_item:hover{
    color: #00b4d4;

  }


</style>

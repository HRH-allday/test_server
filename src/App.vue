<template>
  <div id="app">

    <div class="ui secondary pointing menu">
      <img id="logo" src="/static/logo.png">
      <div class="right menu">
        <a id="menu_item" class="active lightBlue item" @click="routing = 'HomePage'">
          Home
        </a>
        <a id="menu_item_demo1" class="item" @click="routing = 'Demo1'">
          Demo1
        </a>
        <a id="menu_item" class="item" @click="routing = 'Demo2'">
          Demo2
        </a>
      </div>
    </div>

    <div id="contents">
      <div id="Homepage" v-if="routing == 'HomePage'">

        <div id="info_area_home" >
          <div id="info_container_home">
            <h1 id="info_title_home">Image-Color Harmony</h1>
            <h4 id="info_description_home">for Aesthetic & Affective Visual Design</h4>
            <div id="start_button_home" class="ui huge primary button" @click="changeRoute">
              Start
            </div>
          </div>
        </div>

        <div id="home_description">
          <h3> Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text Body text </h3>
        </div>

      </div>

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
      changeRoute : function(){
        this.routing = "Demo1";

        $('#menu_item_demo1').addClass('active lightBlue').siblings().removeClass('active lightBlue');
        $('.ui.modal.scrolling.transition.hidden').remove();
      }
    },
    mounted: function(){

      $(document).ready(function(){
        $('.ui.secondary.pointing.menu a.item').on('click', function(){
          $(this).addClass('active lightBlue').siblings().removeClass('active lightBlue');
          $('.ui.modal.scrolling.transition.hidden').remove();

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
    margin-bottom: 0px;
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

  #menu_item_demo1:hover{
    color: #00b4d4;
  }


  #info_area_home{
    padding-top: 100px;
    padding-bottom: 0px;
    margin-bottom: 0px;
    background-image: url('../static/background.jpeg');
    background-size: 100% 100%;
    height: 450px;
  }

  #info_title_home{
    font-size: 40px;
    font-weight: 700;
    color: #ffffff;
  }

  #info_description_home{
    margin-top: 0px;
    padding-top: 0px;
    font-size: 25px;
    font-weight: 300;
    margin-bottom: 30px;
    color: #ffffff;

  }

  #start_button_home{
    background-color: transparent;
    border: 3px solid;
    border-radius: 5px;
    width: 200px;
    font-size: 25px;
    height: 50px;
    padding-top: 10px;
  }

  #home_description{
    padding: 50px;
    text-align: left;
    margin-top: 30px;
  }


</style>

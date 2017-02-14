<template>
  <div id="app">
    <div class="ui brown secondary pointing menu">
      <p class="item">
        LOGO
      </p>
      <div class="right menu">
        <a class="white active item">
          Home
        </a>
      </div>
    </div>

    <div id="info_area">
      <div id="info_container">
        <p id="info_title">Something Special Happens..</p>
        <div class="ui huge primary button" @click="showModal">
          Get Started
          <i class="right arrow icon"></i>
        </div>
      </div>
    </div>


    <div id="addNewPhoto" class="ui modal">
      <i class="close icon"></i>
      <div class="header">
        New Photo
      </div>
      <div class="content">
        <div class="description">
          <div class="ui header">
            Upload your photo for magical improvement!
          </div>
          <div class="content">
            <div class="ui form">
              <div class="field">
                <label>제목 </label>
                <input class="fluid" v-model="card_title"></input>
              </div>
              <div class="field">
                <label>설명 </label>
                <textarea class="fluid" v-model="card_description" rows="20"></textarea>
              </div>

              <div class="field">
                <label>사진 첨부 </label>
                <div class="ui file input action">
                  <input type="text" id="numOfFiles" readonly v-model="card_photo_title">
                  <input type="file" ref="fileUploads" @change="onFileChangePhoto($event)" style="display: none">
                  <div class="ui button" @click="onSelectFileClick">
                    파일 선택
                  </div>
                </div>

              </div>
              <input type="color" id="html5colorpicker" @change="getColor($event)" value="#ff0000" style="width:85%;">
            </div>
          </div>
        </div>

      </div>

      <div class="actions">
        <div class="ui black deny button">
          Cancel
        </div>
        <div class="ui positive right labeled icon button" @click="uploadImage">
          Ready
          <i class="checkmark icon"></i>
        </div>
      </div>
    </div>


    <div id="card_area">
      <div class="ui link cards">
        <div class="ui card"
        v-for="(card, index) in cardlist" :key="card.id"
        @click="showDetail(card.title, card.description, card.image)">
        <div id="card_preview" class="image"
        v-bind:style="{ 'background-image': 'url(' + card.image + ')' }">
        <img/>
      </div>
      <div class="content">
        <a class="header">{{card.title}}</a>
        <div class="meta">
          <span class="date">{{card.date}}</span>
        </div>
        <div class="description">
          {{card.description}}
        </div>
      </div>
    </div>
  </div>
</div>

</div>
</template>

<script>
  /* eslint-disable */

  var Card = function(image, title, date, description){
    this.id = this.cid++;
    this.image = image || '/static/default.png';
    this.title = title;
    this.date = date;
    this.description = description;
  }

  export default {
    name: 'app',
    data () {
      return {
        cardlist: [new Card('/static/default.png','Hyojoo Han', 'Feb. 10. 2017', 'This is a photo of Korean actress Hyojoo Han.'), new Card('/static/hhj1.jpg','Hyojoo Han', 'Feb. 10. 2017', 'This is a photo of Korean actress Hyojoo Han.'), new Card('/static/hhj2.jpg','Hyojoo Han', 'Feb. 10. 2017', 'This is a photo of Korean actress Hyojoo Han.')],
        cid: 0,
        card_title: "",
        card_description: "",
        card_photo_title: "",
        card_photo: "",
        rgb: "",
        rgbhex: ""
      }
    },
    methods:{
      getImages: function(){
        var httpPost = new XMLHttpRequest(),
        path = "http://143.248.250.191:3000/getImages/",
        data = JSON.stringify({});
        httpPost.onreadystatechange = function(err) {
          if (httpPost.readyState == 4 && httpPost.status == 200){
            var res = httpPost.responseText;
            var jsonList = JSON.parse(res);
            this.cardlist = jsonList;
            console.log(res);

          } else {
            console.log(err);
          }
        }.bind(this);
        // Set the content type of the request to json since that's what's being sent
        httpPost.open("POST", path, true);
        httpPost.setRequestHeader('Content-Type', 'application/json');
        httpPost.send(data);
      },
      uploadImage: function(){
        var httpPost = new XMLHttpRequest(),
        path = "http://143.248.250.191:3000/uploadImage/",
        data = JSON.stringify({title: this.card_title, description: this.card_description, image: this.card_photo, rgb: this.rgb });
        httpPost.onreadystatechange = function(err) {
          if (httpPost.readyState == 4 && httpPost.status == 200){
            var res = httpPost.responseText;
            var jsonList = JSON.parse(res);
            this.cardlist = jsonList;
            this.card_title = "";
            this.card_description = "";
            this.card_photo_title = "";
            this.card_photo = "";

          } else {
            console.log(err);
          }
        }.bind(this);
        // Set the content type of the request to json since that's what's being sent
        httpPost.open("POST", path, true);
        httpPost.setRequestHeader('Content-Type', 'application/json');
        httpPost.send(data);
      },


      showModal: function(){
        $('#addNewPhoto').modal({
          onHide: function(){
            console.log('hidden');
            this.card_title = "";
            this.card_description = "";
            this.card_photo_title = "";

          }.bind(this),
          onShow: function(){
            this.card_title = "";
            this.card_description = "";
            this.card_photo_title = "";
            console.log('shown');
          },
          onApprove: function() {
            console.log('Approve');
          }
        }).modal('show');
      },

      showDetail: function(title, detail,photo_url){
        console.log(title +"," +detail);
        $('#seeDetail').modal({
          onHide: function(){
            console.log('hidden');
            this.card_title = "";
            this.card_description = "";
            this.card_photo_title = "";

          }.bind(this),
          onShow: function(){
            this.card_title = title;
            this.card_description = detail;
            $('#detail_image').attr('src', photo_url);
            console.log('shown');
          }.bind(this),
          onApprove: function() {
            console.log('Approve');
          }
        }).modal('show');
      },

      onSelectFileClick: function () {
        this.$refs.fileUploads.accept = "image/*";
        this.$refs.fileUploads.click();
      },
      onFileChangePhoto(e) {
        var files = e.target.files || e.dataTransfer.files;
        if (!files.length) return;
        this.card_photo_title = files[0].name;
        console.log(files);
        this.createImage(files[0]);
      },
      getColor(e){
        var color = e.target.value
        var value = this.hexToRgb(color);
        console.log(value);
        this.rgb = value;
      },
      hexToRgb(hex) {
        var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
        return result ? {
          r: parseInt(result[1], 16),
          g: parseInt(result[2], 16),
          b: parseInt(result[3], 16)
        } : null;
      },
      createImage(file, index) {
        var image = new Image();
        var reader = new FileReader();
        reader.onload = (e) => {
          this.card_photo = e.target.result;
        };
        reader.readAsDataURL(file);
      },
    },
    mounted: function(){
      this.getImages();
    }
  }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }

  #info_area{
    padding-left: 50px;
    padding-right: 50px;
    padding-top: 30px;
    text-align: left;
  }

  #info_container{
    background-color: #e0e0e0;
    padding: 50px;

  }

  #info_title{
    font-size: 40px;
    font-weight: 700;
    font-family: 'Roboto';
  }

  .ui.secondary.pointing.menu{
    font-size: 18px;
    padding-left: 50px;
    padding-right: 50px;
    height: 25px;
  }

  .ui.huge.primary.button{

  }


  #card_area{
    margin: 50px;
  }

  #card_preview{
    width: 290px;
    height: 300px;
    background-image: url('./assets/hhj1.jpg');
    background-size: 100% 100%;
  }

  .content{
    text-align: left;
  }



</style>

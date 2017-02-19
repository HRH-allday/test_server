<template>
  <div class="demo1">
    <div id="info_area">
      <div id="info_container">
        <h1 id="info_title">Image-Color Harmony</h1>
        <h4 id="info_description">Image Color Adjustment for Harmony with a Color</h4>
        <div class="ui huge primary button" @click="showModal">
          Add New Post
        </div>
      </div>
    </div>


    <div id="addNewPhoto" class="ui modal" style="min-width: 680px;">
      <i class="close icon"></i>
      <div class="header">
        New Photo
      </div>
      <div class="content">
        <div class="description">
          <div class="ui header">
            Upload your photo for magical improvement!
          </div>
          <div class="content" >
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
                  <input type="text" v-model="card_photo_title">
                  <input type="file" id="photoUpload" ref="fileUploads" @change="onFileChangePhoto($event)" style="display: none">
                  <div class="ui button" @click="onSelectFileClick">
                    파일 선택
                  </div>
                </div>

              </div>
              <div class="field">
                <label>색상 선택</label>
                <input id="colorInput" type="color" ref="colorPicker" @change="getColor($event)" value="#ff0000" style="display: none">
                <div id="colorExample">
                </div>
                <div style="float:right; width: 40%;">
                  <div id="colorPickButton" class="ui button" @click="onSelectColorClick" style="float:right; margin-right: 0px;">
                    색상 선택
                  </div>

                  <input id="rgbInput" class="fluid" v-model="rgbhex" style="float:right; width:60%"></input>
                </div>
              </div>
            </div>
          </div>
        </div>

      </div>

      <div class="actions" style="clear:both;">
        <div class="ui black deny button">
          Cancel
        </div>
        <div class="ui positive right labeled icon button" style="background-color: #00b4d4;" @click="uploadImage">
          Ready
          <i class="checkmark icon" style="background-color: #00a0b9;"></i>
        </div>
      </div>

    </div>

    <div id="seeDetail" class="ui modal">
      <i class="close icon"></i>
      <div id="detailHeader" class="header" >
        <h1 id="detailHeaderTitle" >{{detail_title}}</h1>
        <h2 id="detailHeaderDescription" >{{detail_date}}</h2>
      </div>

      <div class="content">
        <div id="cardView" class="ui link centered cards">

          <div>
            <div class="ui card">
              <div id="card_preview_detail" class="image"
              v-bind:style="{ 'background-image': 'url(' + detail_photo + '_output_0.jpg)' }">
            </div>



          </div>

          <div style="text-align: center;">
            <h3 id="card_summary">Original</h3>
          </div>
        </div>

        <div>
          <div class="ui card" style="margin-left: 30px;">
            <div id="card_preview_detail" class="image"
            v-bind:style="{ 'background-image': 'url(' + detail_photo + '_output_1.jpg)' }">
          </div>

        </div>


        <div style="text-align: center;">
          <h3 id="card_summary" style="margin-left: 30px;">Adjusted</h3>
        </div>
      </div>

    </div>

    <div id="detailDescription">
      {{detail_description}}
    </div>
  </div>

  <div class="actions" style="clear:both;">
    <div class="ui positive icon button" style="background-color: #000000; width: 100px;" @click="deleteImage">
      Delete
    </div>
  </div>


</div>


<div id="card_area" class="ui centered">

  <div class="ui link centered cards">
    <div class="ui card"
    v-for="(card, index) in cardlist" :key="card.id"
    @click="showDetail(card.title, card.description, card.image, card.color, card.date, card._id)">
    <div id="card_preview" class="image"
    v-bind:style="{ 'background-image': 'url(' + card.image + '_output.jpg)' }">
  </div>

  <div id="card_text" class="content" v-bind:style="{ 'background-color': card.color }">
    <a id="card_text_title" class="header">{{card.title}}</a>
    <div class="meta">
      <span id="card_text_date" class="date">{{card.date}}</span>
    </div>

    <div id="card_text_description" class="description">
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
    this.image = image || '../static/default.png';
    this.title = title;
    this.date = date;
    this.description = description;
  }

  export default {
    name: 'Demo1',
    data () {
      return {
        cardlist: "",
        cid: 0,
        card_title: "",
        card_description: "",
        card_photo_title: "",
        card_photo: "",
        rgb: {r:255, g:120 ,b:143},
        rgbhex: "#ff788f",
        detail_title: "",
        detail_description: "",
        detail_photo: "",
        detail_date: "",
        detail_color: "",
        detail_id: ""
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
            this.cardlist = jsonList.reverse();
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
        data = JSON.stringify({title: this.card_title, description: this.card_description, image: this.card_photo, rgb: this.rgb, rgbhex: this.rgbhex });
        httpPost.onreadystatechange = function(err) {
          if (httpPost.readyState == 4 && httpPost.status == 200){
            var res = httpPost.responseText;
            var jsonList = JSON.parse(res);
            this.cardlist = jsonList.reverse();
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
      deleteImage: function(){
        var httpPost = new XMLHttpRequest(),
        path = "http://143.248.250.191:3000/deleteImage/",
        data = JSON.stringify({id : this.detail_id});
        httpPost.onreadystatechange = function(err) {
          if (httpPost.readyState == 4 && httpPost.status == 200){
            var res = httpPost.responseText;
            var jsonList = JSON.parse(res);
            this.cardlist = jsonList.reverse();

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
            document.getElementById("photoUpload").value = "";

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

      showDetail: function(title, detail,photo_url, color, date, id){
        console.log(title +"," +detail);
        $('#seeDetail').modal({
          onHide: function(){
            console.log('hidden');
            this.detail_title = "";
            this.detail_description = "";
            this.detail_photo = "";
            this.detail_color = "";
            this.detail_id = "";

          }.bind(this),
          onShow: function(){
            this.detail_title = title;
            this.detail_description = detail;
            this.detail_photo = photo_url;
            this.detail_color = color;
            this.detail_id = id;
            $('#detailHeader').css("background-color", this.detail_color);
            this.detail_date = date;
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
      onSelectColorClick: function () {
        this.$refs.colorPicker.click();
      },
      onFileChangePhoto(e) {
        var files = e.target.files || e.dataTransfer.files;
        if (!files.length) return;
        this.card_photo_title = files[0].name;
        console.log(files);
        this.createImage(files[0]);
      },
      getColor(e){
        this.rgbhex = e.target.value;
        var value = this.hexToRgb(this.rgbhex);
        console.log(value);
        this.rgb = value;
        $('#colorExample').css("background-color", this.rgbhex);
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

  #info_area{
    padding-top: 100px;
    padding-bottom: 0px;
    margin-bottom: 0px;
  }

  #info_title{
    font-size: 40px;
    font-weight: 700;
  }

  #info_description{
    margin-top: 0px;
    padding-top: 0px;
    font-size: 30px;
    font-weight: 300;
    margin-bottom: 30px;

  }


  .ui.huge.primary.button{
    margin: auto;
    background-color: #00b4d4;

  }


  #card_area{
    margin: 50px;
  }

  #card_preview{
    width: 290px;
    height: 300px;
    background-image: url('./../assets/hhj1.jpg');
    background-size: 100% 100%;
  }

  #card_preview_detail{

    width: 290px;
    height: 420px;
  }

  .content{
    text-align: left;
  }

  #rgbInput{
    margin-left: 0px !important;
  }


  #colorExample{
    float: left;
    width: 60% !important;
    height: 35px;
    background-color: #ff788f;
    border-radius: 5px;

  }

  #card_text{
    text-align: center;
    background-color: #e2e2e2;
  }

  #card_text_title{
    color: #ffffff;
    font-size: 23px;

  }

  #card_text_description{
    color: #ffffff;
    font-size: 18px;
  }

  #card_text_date{
    color: #ffffff;

  }

  #detailHeader{
    text-align: center;
    height: 115px;
    color: white;
  }

  #detailHeaderTitle{
    padding-top: 20px;
    vertical-align: middle;
    padding-bottom: 0px;
    margin-bottom: 0px;
    font-size: 29px;
  }

  #detailHeaderDescription{
    padding-top: 0px;
    margin-top: 0px;
    font-size: 17px;
    font-weight: 200;
  }

  #cardView{
    padding-top: 70px;
  }

  #card_summary{
    font-size: 20px;
    font-weight: 100;
  }

  #detailDescription{
    padding: 30px;
    margin-top: 30px;
  }


</style>
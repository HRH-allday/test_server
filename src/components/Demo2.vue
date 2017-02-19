<template>
  <div class="demo2">
    <div id="info_area">
      <div id="info_container">
        <h1 id="info_title">Image-Color Harmony</h1>
        <h4 id="info_description">Color Selection for Harmony with an Image</h4>
        <div class="ui huge primary button" @click="showModal2">
          Add New Post
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
                <input class="fluid" v-model="card_title2"></input>
              </div>
              <div class="field">
                <label>설명 </label>
                <textarea class="fluid" v-model="card_description2" rows="20"></textarea>
              </div>

              <div class="field">
                <label>사진 첨부 </label>
                <div class="ui file input action">
                  <input type="text" v-model="card_photo_title2">
                  <input type="file" ref="fileUploads2" @change="onFileChangePhoto($event)" style="display: none">
                  <div class="ui button" @click="onSelectFileClick2">
                    파일 선택
                  </div>
                </div>

              </div>
            </div>
          </div>
        </div>

      </div>

      <div class="actions">
        <div class="ui black deny button">
          Cancel
        </div>
        <div class="ui positive right labeled icon button" style="background-color: #00b4d4;" @click="uploadImage2">
          Ready
          <i class="checkmark icon" style="background-color: #00a0b9;"></i>
        </div>
      </div>

    </div>

    <div id="seeDetail2" class="ui modal">
      <i class="close icon"></i>
      <div id="detailHeader2" class="header" >
        <h1 id="detailHeaderTitle2" >{{detail_title2}}</h1>
        <h2 id="detailHeaderDescription2" >{{detail_date2}}</h2>
      </div>

      <div class="content">
        <div id="cardView" class="ui link centered cards">

          <div id="detail_card_2" class="ui card">
            <div id="card_preview2" class="image"
            v-bind:style="{ 'background-image': 'url(' + detail_photo2 + '_output_1.jpg)' }">
          </div>


        </div>


        <div id="detail_card_2" class="ui card" >
          <div id="card_preview2" class="image"
          v-bind:style="{ 'background-image': 'url(' + detail_photo2 + '_output_2.jpg)' }">
        </div>

      </div>

      <div id="detail_card_2" class="ui card">
        <div id="card_preview2" class="image"
        v-bind:style="{ 'background-image': 'url(' + detail_photo2 + '_output_3.jpg)' }">
      </div>


    </div>
    <div id="detail_card_2" class="ui card">
      <div id="card_preview2" class="image"
      v-bind:style="{ 'background-image': 'url(' + detail_photo2 + '_output_4.jpg)' }">
    </div>


  </div>
  <div id="detail_card_2" class="ui card">
    <div id="card_preview2" class="image"
    v-bind:style="{ 'background-image': 'url(' + detail_photo2 + '_output_5.jpg)' }">
  </div>


</div>



</div>

<div id="detailDescription">
  {{detail_description2}}
</div>
</div>

  <div class="actions" style="clear:both;">
    <div class="ui positive icon button" style="background-color: #000000; width: 100px;" @click="deleteImage2">
      Delete
    </div>
  </div>



</div>


<div id="card_area" class="ui centered">
  <div class="ui link centered cards">
    <div class="ui card"
    v-for="(card, index) in cardlist2" :key="card.id"
    @click="showDetail2(card.title, card.description, card.image, card.bestColor, card.date, card._id)">
    <div id="card_preview" class="image"
    v-bind:style="{ 'background-image': 'url(' + card.image + '_crop.jpg)' }">
    <img/>
  </div>
  <div id="card_text" class="content" v-bind:style="{ 'background-color': card.bestColor }">
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
    name: 'Demo2',
    data () {
      return {
        cardlist2: [new Card('../static/default.png','Hyojoo Han', 'Feb. 10. 2017', 'This is a photo of Korean actress Hyojoo Han.'), new Card('../static/hhj1.jpg','Hyojoo Han', 'Feb. 10. 2017', 'This is a photo of Korean actress Hyojoo Han.'), new Card('../static/hhj2.jpg','Hyojoo Han', 'Feb. 10. 2017', 'This is a photo of Korean actress Hyojoo Han.')],
        cid2: 0,
        card_title2: "",
        card_description2: "",
        card_photo_title2: "",
        card_photo2: "",
        detail_title2: "",
        detail_description2: "",
        detail_photo2: "",
        detail_date2: "",
        detail_color2: "",
        detail_id2: ""
      }
    },
    methods:{
      getImages2: function(){
        var httpPost = new XMLHttpRequest(),
        path = "http://143.248.250.191:3000/getImages2/",
        data = JSON.stringify({});
        httpPost.onreadystatechange = function(err) {
          if (httpPost.readyState == 4 && httpPost.status == 200){
            var res = httpPost.responseText;
            var jsonList = JSON.parse(res);
            this.cardlist2 = jsonList.reverse();
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
      uploadImage2: function(){
        var httpPost = new XMLHttpRequest(),
        path = "http://143.248.250.191:3000/uploadImage2/",
        data = JSON.stringify({title: this.card_title2, description: this.card_description2, image: this.card_photo2 });
        httpPost.onreadystatechange = function(err) {
          if (httpPost.readyState == 4 && httpPost.status == 200){
            var res = httpPost.responseText;
            var jsonList = JSON.parse(res);
            this.cardlist2 = jsonList.reverse();
            this.card_title2 = "";
            this.card_description2 = "";
            this.card_photo_title2 = "";
            this.card_photo2 = "";

          } else {
            console.log(err);
          }
        }.bind(this);
        // Set the content type of the request to json since that's what's being sent
        httpPost.open("POST", path, true);
        httpPost.setRequestHeader('Content-Type', 'application/json');
        httpPost.send(data);
      },

      deleteImage2: function(){
        var httpPost = new XMLHttpRequest(),
        path = "http://143.248.250.191:3000/deleteImage2/",
        data = JSON.stringify({id : this.detail_id2});
        httpPost.onreadystatechange = function(err) {
          if (httpPost.readyState == 4 && httpPost.status == 200){
            var res = httpPost.responseText;
            var jsonList = JSON.parse(res);
            this.cardlist2 = jsonList.reverse();

          } else {
            console.log(err);
          }
        }.bind(this);
        // Set the content type of the request to json since that's what's being sent
        httpPost.open("POST", path, true);
        httpPost.setRequestHeader('Content-Type', 'application/json');
        httpPost.send(data);
      },


      showModal2: function(){
        $('#addNewPhoto').modal({
          onHide: function(){
            console.log('hidden');
            this.card_title2 = "";
            this.card_description2 = "";
            this.card_photo_title2 = "";

          }.bind(this),
          onShow: function(){
            this.card_title2 = "";
            this.card_description2 = "";
            this.card_photo_title2 = "";
            console.log('shown');
          },
          onApprove: function() {
            console.log('Approve');
          }
        }).modal('show');
      },

      showDetail2: function(title, detail,photo_url, color, date, id){
        console.log(title +"," +detail);
        $('#seeDetail2').modal({
          onHide: function(){
            console.log('hidden');
            this.detail_title2 = "";
            this.detail_description2 = "";
            this.detail_photo2 = "";
            this.detail_color2 = "";
            this.detail_id2 = "";

          }.bind(this),
          onShow: function(){
            this.detail_title2 = title;
            this.detail_description2 = detail;
            this.detail_photo2 = photo_url;
            this.detail_color2 = color;
            this.detail_id2 = id;
            $('#detailHeader2').css("background-color", this.detail_color2);
            this.detail_date2 = date;
            console.log('shown');
          }.bind(this),
          onApprove: function() {
            console.log('Approve');
          }
        }).modal('show');
      },

      onSelectFileClick2: function () {
        this.$refs.fileUploads2.accept = "image/*";
        this.$refs.fileUploads2.click();
      },
      onFileChangePhoto(e) {
        var files = e.target.files || e.dataTransfer.files;
        if (!files.length) return;
        this.card_photo_title2 = files[0].name;
        console.log(files);
        this.createImage(files[0]);
      },
      getColor(e){
        this.rgbhex2 = e.target.value;
        var value = this.hexToRgb(this.rgbhex);
        console.log(value);
        this.rgb2 = value;
        $('#colorExample').css("background-color", this.rgbhex2);
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
          this.card_photo2 = e.target.result;
        };
        reader.readAsDataURL(file);
      },
    },
    mounted: function(){
      this.getImages2();
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

  #card_preview2{
    width: 145px;
    height: 210px;
    background-image: url('./../assets/hhj1.jpg');
    background-size: 100% 100%;
  }

  .content{
    text-align: left;
  }


  #detailHeader2{
    text-align: center;
    height: 115px;
    color: white;
  }

  #detailHeaderTitle2{
    padding-top: 20px;
    vertical-align: middle;
    padding-bottom: 0px;
    margin-bottom: 0px;
    font-size: 29px;
  }

  #detailHeaderDescription2{
    padding-top: 0px;
    margin-top: 0px;
    font-size: 17px;
    font-weight: 200;
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

  #detail_card_2{
    width: 145px;
  }


</style>
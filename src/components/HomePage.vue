<template>
  <div class="homepage">
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
    name: 'HomePage',
    data () {
      return {
        cardlist: [new Card('../static/default.png','Hyojoo Han', 'Feb. 10. 2017', 'This is a photo of Korean actress Hyojoo Han.'), new Card('../static/hhj1.jpg','Hyojoo Han', 'Feb. 10. 2017', 'This is a photo of Korean actress Hyojoo Han.'), new Card('../static/hhj2.jpg','Hyojoo Han', 'Feb. 10. 2017', 'This is a photo of Korean actress Hyojoo Han.')],
        cid: 0,
        card_title: "",
        card_description: "",
        card_photo_title: "",
        card_photo: "",
        rgb: {r:255, g:120 ,b:143},
        rgbhex: "#ff788f"
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

      $(document).ready(function(){
        $('.ui.secondary.pointing.menu a.item').on('click', function(){
          $(this).addClass('active lightBlue').siblings().removeClass('active lightBlue');
        });
      });
    }
  }
</script>

<style>


  #card_area{
    margin: 50px;
  }

  #card_preview{
    width: 290px;
    height: 300px;
    background-image: url('../assets/hhj1.jpg');
    background-size: 100% 100%;
  }

  .content{
    text-align: left;
  }

  #rgbInput{
    float: left;
    width: 25%;
    margin-left: 19px;
  }



  #colorInput{
    float: left;
    margin-top: 5px;
  }

  #colorExample{
    float: left;
    width: 490px;
    height: 35px;
    background-color: #ff788f;
    border-radius: 5px;

  }



</style>
<template>
  <div class="hello">
    <h1>Underline</h1>
    <input type="file" @change="processFile($event)">
    <input type="file" accept="image/*" capture="camera" @change="processFile($event)" />
    <div class="image-preview" v-if="imageData.length > 0">
        <img class="preview" :src="imageData" style="width:400px">
    </div>
    <p v-html="msg"></p>
    <div class="loader loader--style2" title="1" v-if="loading">
      <svg version="1.1" id="loader-1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
        width="40px" height="40px" viewBox="0 0 50 50" style="enable-background:new 0 0 50 50;" xml:space="preserve">
      <path fill="#000" d="M25.251,6.461c-10.318,0-18.683,8.365-18.683,18.683h4.068c0-8.071,6.543-14.615,14.615-14.615V6.461z">
        <animateTransform attributeType="xml"
          attributeName="transform"
          type="rotate"
          from="0 25 25"
          to="360 25 25"
          dur="0.6s"
          repeatCount="indefinite"/>
        </path>
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  name: 'hello',
  data () {
    return {
      msg: '',
      imageData: '',
      loading: false
    }
  },
  methods: {
    processFile: function (event) {
      var that = this
      var input = event.target
      this.loading = true
      this.msg = ''
      // Ensure that you have a file before attempting to read it
      if (input.files && input.files[0]) {
        // create a new FileReader to read this image and convert to base64 format
        var reader = new FileReader()
        // Define a callback function to run, when FileReader finishes its job
        reader.onload = (e) => {
          // Note: arrow function used here, so that "this.imageData" refers to the imageData of Vue component
          // Read image as base64 and set to imageData
          that.imageData = e.target.result
        }
        // Start the reader job - read file as a data url (base64 format)
        reader.readAsDataURL(input.files[0])
      }
      window.Tesseract.recognize(event.target.files[0], {
        lang: 'eng'
      })
        .progress(function (data) {
          console.log(data)
        })
        .then(function (data) {
          console.log(data)
          that.msg = data.html
          that.loading = false
        })
    }
  }
}
</script>
<style scoped>
h1, h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>

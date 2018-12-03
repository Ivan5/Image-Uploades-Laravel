<template>
  <div class="uploader">
    <i class="fas fa-cloud-upload-alt"></i>
    <p>Drag your images here</p>
    <div>OR</div>
    <div class="file-input">
      <label for="file">Select a file</label>
      <input type="file" id="file" multiple @change="onInputChange">
    </div>
  </div>
</template>

<script>
export default {
  methods:{
    onInputChange(e){
      console.log(e);
      
    }
  }
}
</script>

<style lang="scss" scoped>
  .uploader{
    width: 100%;
    background: #2196F3;
    color: #fff;
    padding: 40px 15px;
    text-align: center;
    border-radius: 10px;
    border: 3px dashed #fff;
    font-size: 20px;
    position: relative;

    i{
      font-size: 85px;
    }

    .file-input{
      width: 200px;
      margin: auto;
      height: 68px;
      position: relative;

      label,
      input{
        background: #fff;
        color: #2196F3;
        width: 100%;
        position: absolute;
        left: 0;
        top: 0;
        padding: 10px;
        border-radius: 4px;
        margin-top: 7px;
        cursor: pointer;
      }

      input{
        opacity: 0;
        z-index: -2;
      }
    }
  }
</style>

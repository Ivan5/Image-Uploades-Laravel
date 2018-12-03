<template>
  <div class="uploader" @dragenter="onDragEnter" @dragleave="onDragLeve" @dragover.prevent @drop="onDrop" :class="{ dragging : isDragging }">
    <div v-show="!images.length">
      <i class="fas fa-cloud-upload-alt"></i>
      <p>Drag your images here</p>
      <div>OR</div>
      <div class="file-input">
        <label for="file">Select a file</label>
        <input type="file" id="file" multiple @change="onInputChange">
      </div>
    </div>
    <div class="images-preview" v-show="images.length">
      <div class="img-wrapper" v-for="(image,index) in images" :key="index">
        <img :src="image" :alt="`Image Uploader ${index}`">
        <div class="details">
          <span class="name" v-text="files[index].name"></span>
          <span class="size" v-text="files[index].size"></span>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      isDragging : false,
      dragCount : 0,
      files: [],
      images:[]
    }
  },
  methods:{
    onDragEnter(e){
      e.preventDefault();
      this.dragCount++;
      this.isDragging = true;
    },
    onDragLeve(e){
      e.preventDefault();
      this.dragCount--;

      if(this.dragCount <= 0){
        this.isDragging = false;
      }
      
    },
    onDrop(e){
      e.preventDefault();
      e.stopPropagation();

      this.isDragging = false;

      const files = e.dataTransfer.files;

      Array.from(files).forEach(file => this.addImage(file));
      
    },
    addImage(file){
      if(!file.type.match('image.*')){
        console.log(`${file.name} is not a image`);
        return;
      }
      this.files.push(file);

      const image = new Image(),
          reader = new FileReader();
      
      reader.onload = (e) => this.images.push(e.target.result);

      reader.readAsDataURL(file);
    },
    onInputChange(e){
      const files = e.target.files;

      Array.from(files).forEach(file => this.addImage(file));
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

    &.dragging {
      background: #fff;
      color:#2196F3;
      border: 3px dashed #2196F3;

      .file-input label{
        background: #2196F3;
        color:#fff;
      }
    }

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

    .images-preview{
      display: flex;
      flex-wrap: wrap;
      margin-top: 20px;

      .img-wrapper{
        width: 160px;
        display: flex;
        flex-direction: column;
        margin: 10px;
        height: 150px;
        justify-content: space-between;
        background: #fff;
        box-shadow: 5px 5px 20px #3e3737;

        img{
          max-height: 115px;
        }

      }
      
        .details{
          font-size: 12px;
          background: #fff;
          color: #000;
          display: flex;
          flex-direction: column;
          align-items: self-start;
          padding: 3px 6px;

          .name{
            overflow: hidden;
            height: 18px;
            
          }
        }
    }
  }
</style>

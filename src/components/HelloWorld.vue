<template>
  <div class="hello">
    <video ref="video" :width="videoWidth" :height="videoHeight"></video>
    <div style="margin-top: 20px">
      <v-btn small elevation="2" @click="takeShot"><v-icon>photo_camera</v-icon></v-btn>
    </div>
    <div style="margin: 0 auto 30px auto; display: flex; justify-content: center; flex-direction: column">
      <canvas style="display: none" ref="canvas"></canvas>
      <!-- <div style="display: flex; justify-content: center; align-items: center; flex-wrap: wrap; padding: 0 20px;" v-if="images.length">
        <div v-for="(image, index) of images" :key="index" style="position: relative; padding: 5px">
          <img width="100" ref="img" :src="image.src" />
          <button @click.stop="deleteImage(index)" style="position: absolute; right: 10px; top: 10px">X</button>
          <div>
            <a :href="image.src" ref="download" download="glorious_selfie.png">Save Photo</a>
          </div>
        </div>
      </div> -->
      <v-sheet
        class="mx-auto"
        elevation="8"
        max-width="800"
      >
        <v-slide-group
          v-model="model"
          class="pa-4"
          active-class="success"
          show-arrows
          style="width: 100%; max-width: 500px; margin: 0 auto"
        >
          <v-slide-item
            v-for="(image, index) in images"
            :key="index"
            v-slot="{ active, toggle }"
          >
            <div style="position: relative; padding: 10px">
                    <img width="100" ref="img" :src="image.src" />
                    <v-btn
                      
                      x-small elevation="2" 
                      @click.stop="deleteImage(index)" 
                      color="#607d8b"
                      class="del-btn"
                      style="position: absolute; z-index: 1000000; right: 12px; top: 12px; color: #ffffff; padding: 0"
                    >
                      <v-icon x-small>close</v-icon>
                    </v-btn>
                    <div>
                      <a :href="image.src" style="color: #607d8b; text-decoration: none" ref="download" download="glorious_selfie.png">Save</a>
                    </div>
                  </div>
            <!-- <v-card
              :color="active ? 'blue' : 'grey lighten-1'"
              class="ma-4"
              style="padding: 10px"
              height="200"
              width="100"
              @click="toggle"
            >
              <v-row
                class="fill-height"
                align="center"
                justify="center"
              >
                <v-scale-transition>
                  <v-icon
                    v-if="active"
                    color="white"
                    size="48"
                    v-text="'close'"
                  ></v-icon> -->
                  <!-- <div style="position: relative; padding: 10px">
                    <img width="100" ref="img" :src="image.src" />
                    <v-btn
                      icon small elevation="2" 
                      @click.stop="deleteImage(index)" 
                      color="white"
                      style="position: absolute; z-index: 1000000; right: 10px; top: 10px"
                    >
                      <v-icon small>close</v-icon>
                    </v-btn>
                    <div>
                      <a :href="image.src" ref="download" download="glorious_selfie.png">Save Photo</a>
                    </div>
                  </div> -->
                <!-- </v-scale-transition>
              </v-row>
            </v-card> -->
          </v-slide-item>
        </v-slide-group>
      </v-sheet>
      
    </div>
    
    
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data() {
    return {
      model: null,
      images: [
      ],
      video: {},
      videoWidth: 0,
      videoHeight: 0
    }
  },
  async mounted() {
    const mediaStream = await navigator.mediaDevices.getUserMedia({video: {
      facingMode: 'environment'
    }})
    this.video = this.$refs.video
    this.video.srcObject = mediaStream
    this.video.addEventListener('resize', () => {
      console.log('play')
      console.dir(this.$refs.video)
      console.dir(JSON.parse(JSON.stringify(this.video.videoWidth)))
      const quantifier = this.video.videoWidth / 240
      this.videoWidth = 320
      this.videoHeight = this.video.videoHeight / quantifier
    })
    this.video.play()
  },
  methods: {
    takeShot() {
      const newImage = {src: ''}
      this.images.push(newImage)
      const canvas = this.$refs.canvas
      const video = this.$refs.video
      const width = this.videoWidth
      const height = this.videoHeight

      const ctx = canvas.getContext('2d')
      
      canvas.width = width
      canvas.height = height
      // ctx.translate(canvas.width, 0);
      // ctx.scale(-1, 1);

      ctx.drawImage(video, 0, 0, width, height)
      newImage.src = canvas.toDataURL('image/png')
      console.log(this.images)
    },
    deleteImage(idx) {
      this.images = this.images.filter((img, index) => index != idx)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
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

.del-btn {
  min-width: 18px !important;
  height: 18px !important;
}
</style>

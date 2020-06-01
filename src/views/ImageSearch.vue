<template>
  <div class="container">
    <h1 class="title">Image Search</h1>
    <br />
    <button class="button" :class="{'is-info':file,'is-danger':!file}" @click="toggle">Toggle Camera</button>
    <section class="columns section" v-if="file">
      <div class="file column is-7">
        <label class="file-label">
          <input
            @change="display"
            class="file-input"
            type="file"
            id="file"
            name="image"
            accept="image/*"
          />
          <span style="width:100%;height:3rem" class="file-cta">
            <span class="file-label">Choose a file…</span>
          </span>
        </label>
      </div>
      <b-button style="height:100%;" class="column button is-primary" @click="submit">Search</b-button>
      <div class="column"></div>
    </section>
    <section v-if="file" class="section has-text-weight-semibold">
      <img class style="max-height:240px" src alt />
      <div class="hero has-text-justified" style="min-height:5rem">
        <h1 class="title is-4">Results will be shown here:</h1>
        <div class>
          <div v-if="res.isDog">
            <p :key="i" v-for="(p,i) in res.pred">
              {{p[0]}}-{{p[1]}}%
              <a
                :href="urlify(p[0])"
                target="_blank"
              >{{ }}- Learn more about {{p[0]}}s here.</a>
            </p>
          </div>

          <p v-else-if="res.pred">Thats not a dog. I think its a {{res.pred}}.</p>
        </div>
      </div>
    </section>

    <div class="section" v-show="!file">
      <video style="height:240px;width:320px" id="player" autoplay></video>
      <canvas v-show="picture" id="canvas" width="320px" height="240px"></canvas>
      <div>
        <button @click="recapture" v-show="picture" class="button is-danger">Re-Capture</button>
        <button @click="cap=true" class="button is-success" id="capture-btn">Capture</button>

        <button v-if="cap" class="button is-primary" @click="submit1">Submit</button>
      </div>
      <h1 class="title is-4">Results will be shown here:</h1>
      <div v-if="res.isDog">
        <p :key="i" v-for="(p,i) in res.pred">
          {{p[0]}}-{{p[1]}}%
          <a
            :href="urlify(p[0])"
            target="_blank"
          >{{ }}- Learn more about {{p[0]}}s here.</a>
        </p>
      </div>

      <p v-else-if="res.pred">Thats not a dog. I think its a {{res.pred}}.</p>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    videoPlayer: "",
    captureButton: "",
    captureButton: "",
    picture: "",
    file: false,
    up: false,
    cap: false,
    res: {}
  }),

  mounted() {
    const videoPlayer = document.querySelector("video");
    const captureButton = document.querySelector("#capture-btn");
    const canvasElement = document.querySelector("canvas");
    navigator.mediaDevices
      .getUserMedia({ video: true })
      .then(function(stream) {
        videoPlayer.srcObject = stream;
        videoPlayer.style.display = "block";
      })
      .catch(function(err) {
        imagePickerArea.style.display = "block";
      });

    captureButton.addEventListener("click", event => {
      function dataURItoBlob(dataURI) {
        var byteString = atob(dataURI.split(",")[1]);

        var mimeString = dataURI
          .split(",")[0]
          .split(":")[1]
          .split(";")[0];

        var ab = new ArrayBuffer(byteString.length);

        var ia = new Uint8Array(ab);

        for (var i = 0; i < byteString.length; i++) {
          ia[i] = byteString.charCodeAt(i);
        }

        var blob = new Blob([ab], { type: mimeString });

        navigator.mediaDevices
          .getUserMedia({ video: true })
          .then(function(stream) {
            stream.track.stop();
          });
        return blob;
      }
      canvasElement.style.display = "block";
      videoPlayer.style.display = "none";
      captureButton.style.display = "none";
      var context = canvasElement.getContext("2d");
      context.drawImage(
        videoPlayer,
        0,
        0,
        canvas.width,
        videoPlayer.videoHeight / (videoPlayer.videoWidth / canvas.width)
      );
      videoPlayer.srcObject.getVideoTracks().forEach(videoTrack => {
        videoTrack.stop();
      });
      this.picture = dataURItoBlob(canvasElement.toDataURL());
      console.log(this.picture);
    });
  },
  methods: {
    toggle() {
      this.res = {};
      this.file = !this.file;
      this.up = !this.up;
    },
    urlify(breed) {
      return `https://en.wikipedia.org/wiki/${breed}`;
    },
    recapture() {
      window.location.reload();
    },
    display(event) {
      this.up = true;
      let fReader = new FileReader();
      console.log(event.target.files);
      fReader.readAsDataURL(event.target.files[0]);
      fReader.onloadend = e => {
        console.log("[Before]", document.getElementsByTagName("img").src);
        document.querySelector("img").src = e.target.result;
        console.log("[After]", document.getElementsByTagName("img").src);
      };
    },
    async submit() {
      if (!this.up) {
        alert("Please choose an image");
        return;
      }
      this.res = {};
      let formData = new FormData();
      let imageFile = document.querySelector("#file");
      formData.append("image", imageFile.files[0]);
      const res = await this.$axios.post("http://localhost:5000/", formData, {
        headers: {
          "Content-Type": "multipart/form-data"
        }
      });
      console.log(res);
      this.res = res.data;
    },
    async submit1() {
      this.res = {};

      let formData = new FormData();
      formData.append("image", this.picture, "caputre.jpg");
      const res = await this.$axios.post("http://localhost:5000/", formData, {
        headers: {
          "Content-Type": "multipart/form-data"
        }
      });
      console.log(res);
      this.res = res.data;
    }
  }
};
</script>

<style>
button {
  margin: 1em;
}
</style>

<template>
  <div id="app">

    <el-radio style="position: absolute;width: 20px;top: 30px;left: 20px;" v-model="datasetRadio"
      label="1">V3C</el-radio>
    <el-radio style="position: absolute;width: 20px;top: 30px;left: 120px;" v-model="datasetRadio"
      label="2">Marine</el-radio>

    <el-input style="position: absolute;width: 180px;top: 80px;left: 20px;" type="textarea" :rows="3"
      placeholder="Input" v-model="textarea" resize="nones">
    </el-input>
    <el-radio style="position: absolute;width: 20px;top: 165px;left: 20px;" v-model="radio" label="1">AVS</el-radio>
    <el-radio style="position: absolute;width: 20px;top: 165px;left: 120px;" v-model="radio" label="2">KIS</el-radio>
    <!-- <el-radio style="position: absolute;width: 20px;top: 155px;left: 100px;" v-model="radio" label="3">KIS-T</el-radio> -->

    <!-- <el-upload style="position: absolute;width: 350px;top: 220px;left: 20px;" class="upload-demo" drag
      action="http://localhost:8050/vbs/image" multiple>
      <i class="el-icon-upload"></i>
      <div class="el-upload__text">Drag the file here, or <em>Click to upload</em></div>
    </el-upload> -->

    <el-row style="position: absolute;width: 350px;top:200px;">
      <el-col :span="8" :offset="2">
        {{ submitStatus }}
      </el-col>
    </el-row>
    <el-row style="position: absolute;width: 350px;top:250px;">
      <el-col :span="8" :offset="2">
        LikeCount: {{ LikeCount }}
      </el-col>
    </el-row>
    <el-row style="position: absolute;width: 400px;top:300px;">
      <el-col :span="8" :offset="2">
        NotLikeCount: {{ NotLikeCount }}
      </el-col>
    </el-row>

    <el-row style="position: absolute;width: 350px;top:550px;">
      <el-col :span="8" :offset="2">
        <el-button @click="showV1" type="warning" size="small">Show V1</el-button>
      </el-col>
    </el-row>

    <el-row style="position: absolute;width: 350px;top:600px;">
      <el-col :span="8" :offset="2">
        <el-button @click="submit" type="success" size="small">Submit</el-button>
      </el-col>
      <el-col :span="4" :offset="0">
        <el-button @click="cleanSubmit" type="success" size="small">Clean</el-button>
      </el-col>
    </el-row>

    <el-row style="position: absolute;width: 350px;top:650px;">
      <el-col :span="8" :offset="2">
        <el-button @click="search" type="primary" size="small">Search</el-button>
      </el-col>
    </el-row>

    <el-row style="position: absolute;width: 350px;top:700px;">
      <el-col :span="8" :offset="2">
        <el-button @click="rerank" type="danger" size="small">Re-Rank</el-button>
      </el-col>
    </el-row>


    <el-main
      style="position: absolute;width: 1280px;height: 380px;left: 240px;top: 20px;border-radius: 10px;border: 2px solid rgb(64, 158, 255);">
      <el-image style="width: 118px; height: 80px; border-style: solid; border-width: 1px; border-color: white"
        v-for="(url, index) in urls" @click="ShowpreviewPic(url, index)" :key="url.shot" :src="url.base64" lazy>
      </el-image>
      <el-dialog :visible.sync="visible" :modal="false" width="40%" v-dialogDrag>
        {{ urlIndex }} LikeShots:{{ LikeCount }}
        {{ shot }}
        <img :src="previewpic" alt="" width="100%" />
        <span slot="footer" class="dialog-footer">
          <el-button @click="getV1InV0" type="warning" size="small">Get V1</el-button>
          <el-button @click="ShowV2" type="warning" size="small">Show V2</el-button>
          <el-button @click="Pre" type="info" size="small">PreV</el-button>
          <el-button @click="Skip" type="info" size="small">Skip</el-button>
          <el-button @click="Like" type="primary" size="small">Like</el-button>
          <el-button @click="NotLike" type="danger" size="small">Not Like</el-button>
        </span>
      </el-dialog>
    </el-main>



    <!-- vision1 -->
    <el-container>
      <el-main
        style="position: absolute;width: 550px;height: 340px;left: 240px;top: 410px;border-radius: 10px;border: 2px solid rgb(64, 158, 255);">
        <el-image style="width: 118px; height: 80px; border-style: solid; border-width: 1px; border-color: white"
          v-for="(url, index) in v1urls" @click="ShowpreviewPicV1(url, index)" :key="url.shot" :src="url.base64" lazy>
        </el-image>
        <el-dialog :visible.sync="visibleV1" :modal="false" width="40%" v-dialogDrag>
          {{ shotV1 }}
          <img :src="previewpicV1" alt="" width="100%" />
          <span slot="footer" class="dialog-footer">
            <el-button @click="PreV1" type="info" size="small">PreV</el-button>
            <el-button @click="SkipV1" type="info" size="small">Skip</el-button>
            <el-button @click="addSubmitV1" type="primary" size="small">Add Submit</el-button>
          </span>
        </el-dialog>
      </el-main>
    </el-container>


    <!-- vision2 -->
    <el-container>
      <el-main
        style="position: absolute;width: 720px;height: 340px;left: 800px;top: 410px;border-radius: 10px;border: 2px solid rgb(64, 158, 255);">
        <el-image style="width: 105px; height: 80px; border-style: solid; border-width: 1px; border-color: white"
          v-for="(url, index) in v2urls" @click="ShowpreviewPicV2(url, index)" :key="url.shot" :src="url.base64" lazy>
        </el-image>
        <el-dialog :visible.sync="visibleV2" :modal="false" width="40%" v-dialogDrag>
          {{ shotV2 }}
          <img :src="previewpicV2" alt="" width="100%" />
          <span slot="footer" class="dialog-footer">
            <el-button @click="getV1" type="warning" size="small">Get V1</el-button>
            <el-button @click="PreV2" type="info" size="small">PreV</el-button>
            <el-button @click="SkipV2" type="info" size="small">Skip</el-button>
            <el-button @click="addSubmitV2" type="primary" size="small">Add Submit</el-button>
          </span>
        </el-dialog>
      </el-main>
    </el-container>



  </div>
</template>

<script>

export default {
  name: "App",
  data() {
    return {
      textarea: "",
      radio: "1",
      datasetRadio: "1",
      urls: [
        // 'https://fuss10.elemecdn.com/a/3f/3302e58f9a181d2509f3dc0fa68b0jpeg.jpeg',
        // 'https://fuss10.elemecdn.com/1/34/19aa98b1fcb2781c4fba33d850549jpeg.jpeg',
      ],
      v1urls: [],
      v2urls: [],
      submitUrls: [],
      kisSubmitSelect: 0,
      visible: false,
      visibleV1: false,
      visibleV2: false,
      previewpic: "",
      previewpicV1: "",
      previewpicV2: "",
      shot: "",
      shotV1: "",
      shotV2: "",
      Likes: [],
      NotLikes: [],
      urlIndex: 0,
      urlIndexV1: 0,
      urlIndexV2: 0,
      LikeCount: 0,
      NotLikeCount: 0,
      submitStatus: "",
    };
  },
  methods: {

    search() {
      this.urls = []
      this.v1urls = []
      this.v2urls = []
      this.submitUrls = []
      this.Likes = []
      this.NotLikes = []
      this.shots = []
      this.LikeCount = 0
      this.NotLikeCount = 0
      this.submitStatus = ""
      this.$axios.post('http://localhost:8050/vbs/search', {
        textInput: this.textarea,
        radioSelect: this.radio,
        datasetRadio: this.datasetRadio
      })
        .then(res => {
          res.data.forEach(element => {
            this.urls.push(element)
          })
        }).catch(err => {
          console.log(err)
        })
    },


    addSubmitV1() {
      this.submitUrls.push(this.shotV1)
      this.visibleV1 = false
      this.kisSubmitSelect = 1
    },


    addSubmitV2() {
      this.submitUrls.push(this.shotV2)
      this.visibleV2 = false
      this.kisSubmitSelect = 2
    },

    submit() {
      this.submitStatus = ""
      this.$axios.post('http://localhost:8050/vbs/submit', {
        textInput: this.textarea,
        submitUrls: this.submitUrls,
        radioSelect: this.radio,
        datasetRadio: this.datasetRadio,
        kisSubmitSelect: this.kisSubmitSelect
      })
        .then(res => {
          this.submitStatus = res.data
        }).catch(err => {
          console.log(err)
        })
      this.submitUrls = []
    },

    cleanSubmit() {
      this.submitUrls = []
    },

    getV1() {
      this.visibleV2 = false
      this.shotV1 = this.shotV2
      this.$axios.post('http://localhost:8050/vbs/getVision1', {
        shot: this.shotV1,
        datasetRadio: this.datasetRadio
      })
        .then(res => {
          res.data.forEach(element => {
            console.log(element);
          })
        }).catch(err => {
          console.log(err)
        })
    },


    getV1InV0() {
      this.visible = false
      this.shotV1 = this.shot
      this.$axios.post('http://localhost:8050/vbs/getVision1', {
        shot: this.shotV1,
        datasetRadio: this.datasetRadio
      })
        .then(res => {
          res.data.forEach(element => {
            console.log(element);
          })
        }).catch(err => {
          console.log(err)
        })
    },

    showV1() {
      this.v1urls = []
      this.$axios.post('http://localhost:8050/vbs/showVision1', {
        shot: this.shotV1,
        datasetRadio: this.datasetRadio
      })
        .then(res => {
          res.data.forEach(element => {
            this.v1urls.push(element)
          })
        }).catch(err => {
          console.log(err)
        })
    },

    ShowpreviewPic(url, index) {
      this.previewpic = url.base64
      this.visible = true;
      this.shot = url.shot
      this.urlIndex = index
    },

    ShowpreviewPicV1(url, index) {
      this.previewpicV1 = url.base64
      this.visibleV1 = true;
      this.shotV1 = url.shot
      this.urlIndexV1 = index
    },

    ShowpreviewPicV2(url, index) {
      this.visibleV2 = true
      this.previewpicV2 = url.base64
      this.shotV2 = url.shot
      this.urlIndexV2 = index
    },

    ShowV2() {
      this.visible = false
      this.v2urls = []
      console.log(this.shot);
      this.$axios.post('http://localhost:8050/vbs/showVideoShot', {
        shot: this.shot,
        datasetRadio: this.datasetRadio
      })
        .then(res => {
          res.data.forEach(element => {
            this.v2urls.push(element)
          })
        }).catch(err => {
          console.log(err)
        })
    },

    Like() {
      this.LikeCount++
      this.shot = this.urls[this.urlIndex].shot
      this.Likes.push(this.shot)
      this.urlIndex = this.urlIndex + 1
      this.previewpic = this.urls[this.urlIndex].base64
      // console.log("like ", this.shot);
    },

    NotLike() {
      this.NotLikeCount++
      this.shot = this.urls[this.urlIndex].shot
      this.NotLikes.push(this.shot)
      this.urlIndex = this.urlIndex + 1
      this.previewpic = this.urls[this.urlIndex].base64
      // console.log("Not like ", this.shot);
    },

    Skip() {
      this.urlIndex = this.urlIndex + 1
      this.previewpic = this.urls[this.urlIndex].base64
    },

    SkipV1() {
      this.urlIndexV1 = this.urlIndexV1 + 1
      this.shotV1 = this.v1urls[this.urlIndexV1].shot
      this.previewpicV1 = this.v1urls[this.urlIndexV1].base64
    },


    SkipV2() {
      this.urlIndexV2 = this.urlIndexV2 + 1
      this.previewpicV2 = this.v2urls[this.urlIndexV2].base64
    },

    Pre() {
      this.urlIndex = this.urlIndex - 1
      this.previewpic = this.urls[this.urlIndex].base64
    },

    PreV1() {
      this.urlIndexV1 = this.urlIndexV1 - 1
      this.previewpicV1 = this.v1urls[this.urlIndexV1].base64
      this.shotV1 = this.v1urls[this.urlIndexV1].shot
    },

    PreV2() {
      this.urlIndexV2 = this.urlIndexV2 - 1
      this.previewpicV2 = this.v2urls[this.urlIndexV2].base64
    },

    rerank() {
      this.LikeCount = 0
      this.NotLikeCount = 0
      this.$axios.post('http://localhost:8050/vbs/reRank', {
        LikePaths: this.Likes,
        NotLikePaths: this.NotLikes,
        datasetRadio: this.datasetRadio
      })
        .then(res => {
          this.urls = []
          console.log(res);
          res.data.forEach(element => {
            this.urls.push(element)
          })
        }).catch(err => {
          console.log(err)
        })
      this.Likes = []
      this.NotLikes = []
    },
  },
};
</script>

<style>
/* .el-main {
  position: absolute;
  width: 1030px;
  height: 470px;
  left: 450px;
  top: 20px;
  border-radius: 10px;
  border: 2px solid rgb(64, 158, 255);
} */
</style>

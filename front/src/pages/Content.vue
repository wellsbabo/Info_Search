<template>
  <v-app>
    <v-app-bar app color="red" dark>
      <v-spacer></v-spacer>
      <v-app-bar-title>
        <div align="center" :style="{fontSize:'xx-large'} ">게시판 {{$route.params.id}}</div>
      </v-app-bar-title>
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-main>
      <v-container>
        <v-row>
          <v-col cols="12" md="1">
            <v-btn color="cyan" @click="movetomain"
                   :style="{height:'50px', width:'90px', fontWeight:'bold', fontSize:'large'}">홈으로</v-btn>
          </v-col>
          <div v-show="is_show" style="width: 300px; margin-left: 100px; padding-top: 20px;">
              <h5>수정</h5>
              <p>수정 모달창</p>
              <button @click="edit_toggle" type="button">확인</button>
          </div>
          <v-col cols="12" md="10">
            <v-card>
              <div style="width: 300px; margin-left: 100px; padding-top: 20px;">IPaddr : {{ipaddr}}</div>
              <div style="width: 300px; margin-left: 100px; padding-top: 10px;">연관정보 : {{gname}}</div>
              <div style="width: 300px; margin-left: 100px; padding-top: 10px;">비고 : {{etc}}</div>
              <div style="width: 300px; margin-left: 100px; padding-top: 10px;">작성일 : {{date}}</div>
              <v-btn width="100px" style="margin-left: 470px; margin-bottom:20px;" @click="moveback">뒤로가기</v-btn>
              <v-btn width="100px" style="margin-left: 30px; margin-bottom:20px;" @click="edit_toggle"
                      v-if="editable===false">수정</v-btn>
              <v-btn width="100px" style="margin-left: 30px; margin-bottom:20px;" @click="editcontentfinish"
                     v-if="editable===true">수정완료</v-btn>
              <v-btn width="100px" style="margin-left: 30px; margin-bottom:20px;" @click="deletecontent">삭제</v-btn>
              
          </v-card>
          </v-col>
          <v-col cols="12" md="1"/>
        </v-row>

      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from 'axios'

export default {
  data(){
    return {
      ipaddr: '',
      gname: '',
      etc: '',
      date: '',
      //text: '',
      editable: false,
      //imagelist: [],
      //imagecnt: 0,
      is_show: false,
    }
  },
  mounted() {
    console.log(this.$route.query.id)
    axios({
      url: "http://172.16.110.29:33997/contentDetail",
      method: "POST",
      data: {
        id: this.$route.query.id
      },
    }).then(res => {
      this.ipaddr = res.data.ipaddr;
      this.gname = res.data.gname;
      this.etc = res.data.etc;
      this.date = res.data.date
    }).catch(err => {
      alert(err);
    });
  },
  methods: {
    moveback() {
      window.history.back();
    },
    edit_toggle:function(){
      this.is_show = !this.is_show;
    },
    deletecontent() {
      axios({
        url: "http://127.0.0.1:52273/content/delete/",
        method: "POST",
        data: {
          id: this.$route.query.id
        },
      }).then(res => {
        alert(res.data.message);
        window.location.href = window.location.pathname.slice(0,-8) + '/?page=1';
      }).catch(err => {
        alert(err);
      });
    },
    editcontent() {
      this.editable = true;
    },
    editcontentfinish() {
      axios({
        url: "http://127.0.0.1:52273/content/edit/",
        method: "POST",
        data: {
          id: this.$route.query.id,
          
        },
      }).then(res => {
        alert(res.data.message);
        this.editable = false;
      }).catch(err => {
        alert(err);
      });
    },
    movetomain() {
      window.location.href='/';
    },

  },
};
</script>
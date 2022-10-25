<template>
  <v-app>
    <v-app-bar app dark>
      <v-spacer></v-spacer>
      <v-app-bar-title>
        <div :style="{fontSize:'xx-large'} ">IP info</div>
      </v-app-bar-title>
      <v-spacer></v-spacer>
    </v-app-bar>
    <v-main>
      <v-container>
        <v-row>
          <v-col cols="12" md="3"></v-col>
<!--           <v-col cols="12" md="2" align="center">
            <v-btn color="light-green" @click="movetoboard1"
                   :style="{height:'50px', width:'170px', fontWeight:'bold', fontSize:'large'}">게시판1</v-btn>
          </v-col> -->
<!--           <v-col cols="12" md="2" align="center">
            <v-btn color="yellow" @click="movetoboard2"
                   :style="{height:'50px', width:'170px', fontWeight:'bold', fontSize:'large'}">게시판2</v-btn>
          </v-col>
          <v-col cols="12" md="2" align="center">
            <v-btn color="orange" type="submit" @click="movetoboard3"
                   :style="{height:'50px', width:'170px', fontWeight:'bold', fontSize:'large'}">게시판3</v-btn>
          </v-col> -->
          <v-col cols="12" md="3"></v-col>
        </v-row>

        <v-row :style="{marginTop: '50px'}">
          <v-col cols="12" md="2" />
          <v-col cols="12" md="2">
          </v-col>
<!--           <v-col cols="12" md="4">
            <v-text-field v-model="searchkeyword" dense outlined label="검색키워드" full-width :style="{marginTop:'10px'}"/>
          </v-col> -->
          <v-col cols="12" md="4">
            <textarea v-model="searchkeyword" dense outlined label="검색키워드" full-width :style="{marginTop:'10px'}"></textarea>
            <v-btn @click="searchstart" :style="{marginTop:'10px'}">검색</v-btn>
          </v-col>
          <v-col cols="12" md="1">
            
          </v-col>
          <v-col cols="12" md="3" />
        </v-row>
        <v-row v-if="searchfinish===true" :style="{marginTop:'0px'}">
          <v-col cols="12" md="5"/>
          <v-col cols="12" md="2">
            <div><p id="result">결과 : {{searchcnt}}개</p></div>
          </v-col>
          <v-col cols="12" md="5"/>
        </v-row>
        <v-row v-if="searchfinish===true">
          <v-simple-table style="width: 100%;">
            <thead>
            <tr style="font-weight: bolder;">
              <td style="width:10%;">IPaddr</td>
              <td style="width:20%;">연관정보</td>
              <td style="width:50%;">비고</td>
              <td style="width:20%;">작성일</td>
            </tr>
            </thead>
            <tbody>
            <tr v-for="item in contentlist" :key="item.id" @click="movetocontent('edit',item._id)">
              <td>{{item.ipaddr}}</td>
              <td>{{item.gname}}</td>
              <td>{{item.etc}}</td>
              <td>{{item.date}}</td>
            </tr>
            </tbody>
          </v-simple-table>
        </v-row>

      </v-container>
    </v-main>
    <v-footer min-height="50px" color="white"/>
  </v-app>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      searchkeyword: '',
      searchfinish: false,
      searchoption: ['제목','작성자'],
      searchoptionselected: '제목',
      searchcnt: 0,
      contentlist: [],
    }
  },
  methods: {
    movetoboard1(){
      window.location.href='/board/1/?page=1'
    },
    movetoboard2(){
      window.location.href='/board/2/?page=1'
    },
    movetoboard3(){
      window.location.href='/board/3/?page=1'
    },
    movetocontent(boardnum,id) {
      //window.location.href = 'http://172.16.110.29:8080/board/' + boardnum + '/content?id=' + id
      console.log(boardnum,id)
      //window.location.href = 'http://172.16.110.29:8080/board/content?id=' + id
    },
    searchstart(){
      if(this.searchkeyword == '') {
        alert('키워드가 없습니다!');
      } else {
        //console.log(this.searchkeyword)
        axios({
          url: "http://172.16.110.29:33997/search",
          method: "POST",
          data: {
            searchData: this.searchkeyword
          },
        }).then(res => {
          console.log(res)
          this.contentlist = res.data;
          //this.searchcnt = this.contentlist[Object.keys(this.contentlist).length-1].cnt;
          this.searchcnt = Object.keys(this.contentlist).length
          if(this.searchcnt == -1){
            this.searchcnt = 0
          }
          //this.contentlist.pop();
          //alert('검색완료!');
          this.searchfinish = true;
          this.searchkeyword = '';
        }).catch(err => {
          alert(err);
        });
      }
    },
  },
};
</script>

<style scoped>
textarea {
	width: 100%;
	height: 100px;
	padding: 10px;
	box-sizing: border-box;
	border: solid 2px #1E90FF;
	border-radius: 5px;
	font-size: 16px;
	resize: both;
  background-color: white;
}

#result{
  color: white;
}
</style>
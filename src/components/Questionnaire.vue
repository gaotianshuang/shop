<template>
  <div>
    <pic-night :position="position" :url="url"></pic-night>
    <ul>
      <li v-if="list.length === 0" class="li-list">暂无数据</li>
      <li v-for="item in list" class="li-list">
        <a :href="'#/QuestionnaireDetail/'+item.id">
          <p class="activity">{{item.vote}}</p>
          <p class="date">{{item.date}}</p>
        </a>
      </li>
    </ul>
  </div>
</template>
<script>
  import axios from 'axios'
  import PicNight from '@/sub/PicNight'
  export default{
    name: 'Questionnaire',
    data () {
      return {
        position: '',
        list: []
      }
    },
    created () {
      this.loadData()
    },
    methods: {
      loadData: function () {
        let that = this
        axios.get('http://rating.pandawork.net/wechat/quest/ajax/list', {
          params: {
            openId: localStorage.getItem('openId')
          }
        }, {
          headers: {
            'Access-Control-Allow-Origin': '*'
          }
        })
          .then((response) => {
            response = response.data
            if (String(response.code) === '0') {
              that.position = response.position
              that.url = 'http://rating.pandawork.net' + response.url
              that.list = response.list
            } else {
              that.$dialog.toast({mes: response.errMsg, timeout: 2000})
            }
          })
          .catch(function (error) {
            that.$dialog.toast({mes: '加载数据异常', timeout: 2000})
            console.log(error)
          })
      }
    },
    components: {
      PicNight
    }
  }
</script>
<style>
  .activity{
    word-wrap: normal;
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;
    text-align: justify;
    height: .46rem;
    color: #505050;
    font-size: .32rem;
    line-height: .32rem;
    text-align: justify;
    font-weight: 800;
    margin-top: .2rem;
  }
  .li-list{
    padding: .15rem;
    background-color: #ffffff;
    margin-bottom: .1rem;
  }
  .date{
    overflow: hidden;
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-align: end;
    -webkit-align-items: flex-end;
    -ms-flex-align: end;
    align-items: flex-end;
    -webkit-box-pack: justify;
    -webkit-justify-content: space-between;
    -ms-flex-pack: justify;
    justify-content: space-between;
    color: #999;
    font-size: .28rem;
  }
</style>

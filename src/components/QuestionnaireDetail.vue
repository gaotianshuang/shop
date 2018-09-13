<template>
  <div>
    <yd-navbar title="问卷调查" bgcolor="#0399BA" color="#ffffff"></yd-navbar>
    <div class="content">
      <p class="title">{{title}}</p>
      <p class="subTitle">发布人：{{position}} 发布时间：{{date}} 截止日期：{{endDate}}</p>
      <br>
      <p class="sel tip">{{subTitle}}</p>
      <br>
      <ul class="list">
        <li v-for="items,indexs in list">
          <template v-if="items.type=='1'">
            <p>{{indexs+1}}.{{items.title}}</p>
            <template v-for="item,index in items.collection">
              <label v-for="ite,ind in item">
                <input type="radio" :name="items.id" :value="ind" v-model="formData[items.id]">&nbsp;{{ite}}<br>
              </label>
            </template>
          </template>
          <template v-if="items.type=='2'">
            <p>{{indexs+1}}.{{items.title}}</p>
            <check-com :items="items" :formData="formData"></check-com>
          </template>
          <template v-if="items.type=='3'">
            <p>{{indexs+1}}.{{items.title}}</p>
            <select :name="items.id" class="select" v-model="formData[items.id]">
              <option :value="undefined" disabled>请选择</option>
              <template v-for="item,index in items.collection">
                <option :value="ind" v-for="ite,ind in item">{{ite}}</option>
              </template>
            </select>
          </template>
          <template v-if="items.type=='4'">
            <p>{{indexs+1}}.{{items.title}}</p>
            <input type="text" v-model.trim="formData[items.id]" class="inputText">
          </template>
          <template v-if="items.type=='5'">
            <p>{{indexs+1}}.{{items.title}}</p>
            <textarea :name="items.id" class="textarea" v-model.trim="formData[items.id]"></textarea>
          </template>
        </li>
      </ul>
      <yd-button size="large" type="primary" @click.native="send" :disabled="dis">提交</yd-button>
    </div>
  </div>
</template>
<script>
  import axios from 'axios'
  import CheckCom from '@/sub/CheckCom'
  export default{
    name: 'QuestionnaireDetail',
    data () {
      return {
        title: '',
        position: '',
        date: '',
        endDate: '',
        subTitle: '',
        list: [],
        formData: {},
        checkbox: [],
        flag: false,
        dis: false
      }
    },
    created () {
      this.loadData()
    },
    components: {
      CheckCom
    },
    methods: {
      loadData: function () {
        let that = this
        axios.get('http://rating.pandawork.net/wechat/quest/toNew', {
          params: {
            id: that.$route.params.id
          }
        }, {
          headers: {
            'Access-Control-Allow-Origin': '*'
          }
        })
          .then((response) => {
            response = response.data
            if (String(response.code) === '0') {
              this.title = response.title
              this.position = response.position
              this.date = response.date
              this.endDate = response.endDate
              this.subTitle = response.subTitle
              this.list = response.list
            } else {
              this.$dialog.toast({mes: response.errMsg, timeout: 2000})
            }
          })
          .catch(function (error) {
            this.$dialog.toast({mes: '加载数据异常', timeout: 2000})
            that.dis = true
            console.log(error)
          })
      },
      send: function (event) {
        let that = this
        let data = []
        let i = 0
        that.flag = false
        for (; i < that.list.length; i++) {
          let item = that.list[i].id
          if (that.formData[item] === undefined || that.formData[item] === '-1' || (that.formData[item] !== undefined && that.formData[item].length < 1)) {
            that.$dialog.toast({mes: '信息填写不完整，请填好后重新提交！', timeout: 2000})
            that.flag = true
            break
          } else if (String(that.list[i].type) === '2') {
            if (that.formData[item] instanceof Array && that.formData[item].length > that.list[i].limit) {
              that.$dialog.toast({mes: '第' + (i + 1) + '题最多可选' + that.list[i].limit + '项，请填好后重新提交！', timeout: 2000})
              that.flag = true
              break
            } else {
              console.log(typeof (that.formData[item]))
              if (typeof that.formData[item] === 'string' && that.formData[item].split(',').length > that.list[i].limit) {
                that.$dialog.toast({mes: '第' + (i + 1) + '题最多可选' + that.list[i].limit + '项，请填好后重新提交！', timeout: 2000})
                that.flag = true
                break
              }
            }
            that.formData[item] = that.formData[item].toString()
          }
        }
        for (let item in that.formData) {
          data.push({questItemId: item, content: that.formData[item]})
        }
        if (that.flag === false) {
          axios.post('http://rating.pandawork.net/wechat/quest/ajax/new', JSON.stringify({
            userId: localStorage.getItem('userId'),
            questId: that.$route.params.id,
            questItemResultList: data
          }), {
            headers: {
              'Access-Control-Allow-Origin': '*',
              'Content-Type': 'application/json'
            }
          })
            .then((response) => {
              response = response.data
              if (String(response.code) === '0') {
                that.$dialog.toast({mes: '提交成功！', timeout: 2000})
                window.location = '#/Questionnaire'
              } else {
                that.$dialog.toast({mes: response.errMsg, timeout: 2000})
              }
            })
            .catch(function (error) {
              that.$dialog.toast({mes: '操作失败！', timeout: 2000})
              console.log(error)
            })
        }
      }
    }
  }
</script>
<style>
  .content{
    padding: .10rem;
  }
  .title{
    color: #0399BA;
    font-size: .40rem;
    line-height: .40rem;
    height: .50rem;
    margin-top: .20rem;
  }
  .subTitle{
    font-size: .30rem;
    padding: 0 .5rem;
  }
  .tip{
    text-indent: .6rem;
  }
  .select{
    text-align: left;
    font-size: .30rem;
    line-height: .55rem;
    margin-bottom: .20rem;
  }
  .select{
    text-align: left;
    font-size: .30rem;
    line-height: .35rem;
    margin-bottom: .20rem;
  }
  .list{
    text-align: left;
    padding: 0 .5rem;
    font-size: .3rem;
    line-height: .5rem;
  }
  .list .textarea{
    padding: .1rem;
    width: 6rem;
    height: 2.5rem;
  }
</style>

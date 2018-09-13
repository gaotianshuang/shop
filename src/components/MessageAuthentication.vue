<template>
  <div>
    <yd-navbar title="信息验证" bgcolor="#0399BA" color="#ffffff"></yd-navbar>
    <yd-cell-group>
      <yd-cell-item>
        <span slot="left">手机号：</span>
        <yd-input slot="right" v-model="number" required :show-required-icon="false" type="number" placeholder="请输入您的学号" ></yd-input>
      </yd-cell-item>
      <yd-cell-item>
        <span slot="left">姓名：</span>
        <yd-input slot="right" v-model="name" required :show-required-icon="false" placeholder="请输入您的姓名"></yd-input>
      </yd-cell-item>
    </yd-cell-group>
    <yd-button size="large" type="primary" @click.native="send">下一步</yd-button>
  </div>
</template>
<script type="text/babel">
  import axios from 'axios'
  export default{
    name: 'MessageAuthentication',
    data () {
      return {
        number: '',
        name: '',
        type: '',
        openid: ''
      }
    },
    created () {
      this.loadData()
    },
    methods: {
      loadData: function () {
        let that = this
        axios.get('http://rating.pandawork.net/wechat/check', {
          params: {
            openId: that.$route.params.openId
          }
        }, {
          headers: {
            'Access-Control-Allow-Origin': '*'
          }
        })
          .then((response) => {
            response = response.data
            if (String(response.code) === '0') {
              let status = response.status
              that.type = that.$route.params.type
              that.openId = that.$route.params.openId
              localStorage.setItem('openId', that.openId)
              localStorage.setItem('userId', response.userId)
              if (String(status) !== '0') {
                window.location = '#/Login/' + that.type
              }
            } else {
              that.$dialog.toast({mes: response.errMsg, timeout: 2000})
            }
          })
          .catch(function (error) {
            that.$dialog.toast({mes: '加载异常', timeout: 2000})
            console.log(error)
          })
      },
      send: function () {
        let that = this
        if (that.number === '' || that.name === '') {
          that.$dialog.toast({mes: '输入信息不能为空，请检查信息重新输入！', timeout: 2000})
        } else {
          axios.get('http://rating.pandawork.net/wechat/validate', {
            params: {
              number: that.number,
              name: that.name
            }
          }, {
            headers: {
              'Access-Control-Allow-Origin': '*'
            }
          })
            .then((response) => {
              response = response.data
              if (String(response.code) === '0') {
                let type = that.type
                // localStorage.setItem('userId', response.userId)
                window.location = '#/SetPassword/' + type
              } else {
                that.$dialog.toast({mes: response.errMsg, timeout: 2000})
              }
            })
            .catch(function (error) {
              that.$dialog.toast({mes: '数据提交失败，请稍后再试！', timeout: 2000})
              console.log(error)
            })
        }
      }
    }
  }
</script>
<style>

</style>

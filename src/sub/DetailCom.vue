<template>
  <yd-flexbox class="border">
    <yd-flexbox-item v-for="items,indexs in list" :key="indexs">
      <template v-if="items.type==1">
        <div class="table"><span>{{items.tableHead}}</span></div>
        <template v-for="item in items.tableBody">
          <div class="table" v-for="ite in item">
            <span>{{ite}}</span>
          </div>
        </template>
      </template>
      <template v-if="items.type==2">
        <div class="table"><span>{{items.tableHead}}</span></div>
        <div class="table" v-for="item in items.tableBody">
          <!--单选-->
          <template v-for="ite,ind in item">
            <input type="radio" :name="items.id" :value="ind" v-model="formData[items.id]"/>
          </template>
        </div>
      </template>
      <template v-if="items.type==3">
        <div class="table"><span>{{items.tableHead}}</span></div>
        <!--<div class="table" v-for="item in items.tableBody">-->
          <!--多选-->
          <annual-check :items="items" :formData="formData"></annual-check>
        <!--</div>-->
      </template>
      <!--<template v-if="items.type==4">-->
        <!--<select-com :items="items" :formData="formData"></select-com>-->
      <!--</template>-->
    </yd-flexbox-item>
  </yd-flexbox>
</template>
<script>
  import selectCom from '@/sub/SelectCom'
  import AnnualCheck from '@/sub/AnnualCheck'
  export default{
    name: 'DetailRadio',
    props: ['list', 'formData'],
    data () {
      return {
        select: [],
        input: []
      }
    },
    components: {
      selectCom,
      AnnualCheck
    }
  }
</script>
<style>
  .border div.table{
    border: .01rem solid  #cccccc;
  }
  .table{
    font-size: .30rem;
    height: .45rem;
  }
</style>

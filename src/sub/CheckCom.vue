<template>
  <div>
    <template v-for="item,index in items.collection">
      <label v-for="ite,ind in item">
        <input type="checkbox" :name="items.id" :value="ind" v-model="checkbox">&nbsp;{{ite}}<br>
      </label>
    </template>
    <span v-show="false">{{formData[items.id] = checkbox}}</span>
  </div>
</template>
<script>
  export default {
    name: 'CheckCom',
    props: ['items', 'formData'],
    data () {
      return {
        checkbox: []
      }
    }
  }
</script>
<style></style>

<template>
  <div>
    <div class="table" v-for="item in items.tableBody">
      <template v-for="ite,ind in item">
        <input type="checkbox" :name="items.id" :value="ind" v-model="check"/>
      </template>
      <span v-show="false">{{formData[items.id] = check}}</span>
    </div>
  </div>
</template>
<script>
  export default {
    name: 'AnnualCheck',
    props: ['items', 'formData'],
    data () {
      return {
        check: []
      }
    }
  }
</script>
<style></style>

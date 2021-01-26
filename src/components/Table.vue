<template>
  <table class="table">
    <thead>
      <slot name="columns">
        <tr>
          <th v-for="column in columns" :key="column">{{column}}</th>
        </tr>
      </slot>
    </thead>
    <tbody>
    <tr v-for="(item, index) in data" :key="index">
      <slot :row="item">
        <td> {{item.name}} </td>
        <td> {{item.territory}}</td>
        <td> {{item.value}} </td>
        <td> {{ formatDate(item.limitToPay) }} </td>
        <td> {{item.link}} </td>
      </slot>
    </tr>
    </tbody>
  </table>
</template>
<script>
  export default {
    name: 'l-table',
    props: {
      columns: Array,
      data: Array
    },
    methods: {
      hasValue (item, column) {
        return item[column.toLowerCase()] !== 'undefined'
      },
      itemValue (item, column) {
        return item[column.toLowerCase()]
      }, 
      formatDate(date) {
        let newDate = new Date(date);
        let dateFormated = newDate.toLocaleDateString('pt-BR', {timeZone: 'UTC'});
        return dateFormated;
      }
    }
  }
</script>
<style>
</style>

<template>
  <div>
    <base-header class="pb-6 pb-8 pt-5 bg-gradient-warning">
      <b-card>
        <b-row>
          <b-col xl="9" md="6">
            <h2 class="mt-2">{{pageTitle}}</h2>
          </b-col>
          
          <b-col xl="3" md="6" align="right">
            <b-button v-on:click="showModal()" variant="warning">
              <i class=" mb-0 ni ni ni-fat-add"></i> 
              novo assunto
            </b-button>
          </b-col>
        </b-row>
      </b-card>
    </base-header>
    <b-container fluid class="mt--7">
      <b-row>
        <b-col>
          <list ref="list"/>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>
<script>
  import { Dropdown, DropdownItem, DropdownMenu, Table, TableColumn } from 'element-ui';
  import List from "./List";

  export default {
    components: {
      List,
      [Dropdown.name]: Dropdown,
      [DropdownItem.name]: DropdownItem,
      [DropdownMenu.name]: DropdownMenu,
      [Table.name]: Table,
      [TableColumn.name]: TableColumn
    },
    data() {
      return {
        pageTitle: 'Assuntos'
      };
    },
    methods: {
      showModal() {
        this.$refs.list.new();
      }
    },
  };
</script>
<style>
.el-table.table-dark{
  background-color: #172b4d;
  color: #f8f9fe;
}

.el-table.table-dark th,
.el-table.table-dark tr{
  background-color: #172b4d;
}

.el-table.table-dark td,
.el-table.table-dark th.is-leaf{
  border-bottom: none;
}
</style>

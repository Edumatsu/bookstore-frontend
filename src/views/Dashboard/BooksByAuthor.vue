<template>

  <b-card body-class="p-0" header-class="border-0">
    <template v-slot:header>
      <b-row align-v="center">
        <b-col>
          <h3 class="mb-0">Livros por autor</h3>
        </b-col>
      </b-row>
    </template>

    <b-row align-v="center">
      <b-col>
        <download-excel :data="tableData">
          <i title="Baixar" class=" mb-0 ni ni-fat-remove text-success"></i> 
        </download-excel>
      </b-col>
    </b-row>

    <el-table class="table-responsive table"
              :data="tableData"
              header-row-class-name="thead-light">
      <el-table-column prop="author" label="Autor">
        <template v-slot="{row}">
          <div class="font-weight-600">{{row.author}}</div>
        </template>
      </el-table-column>
      <el-table-column prop="book" label="Livro">
        <template v-slot="{row}">
          <div class="text-break">{{row.book}}</div>
        </template>
      </el-table-column>
      <el-table-column prop="publishingCompany" label="Editora" class="text-wrap"></el-table-column>
      <el-table-column prop="edition" label="Edição" class="text-wrap"></el-table-column>
      <el-table-column prop="yearPublication" label="Ano" class="text-wrap"></el-table-column>
      <el-table-column prop="value" label="Valor" class="text-wrap">
        <template v-slot="{row}">
          <div class="text-break">{{formatPrice(row.value)}}</div>
        </template>
      </el-table-column>
      <el-table-column prop="subjects" label="Assuntos" class="text-wrap"></el-table-column>
    </el-table>
  </b-card>
</template>
<script>
  import axios from '../../plugins/axios'
  import { Table, TableColumn, DropdownMenu, DropdownItem, Dropdown} from 'element-ui'
  const BRL = new Intl.NumberFormat('pt-BR', {
      style: 'currency',
      currency: 'BRL',
  });

  export default {
    name: 'page-visits-table',
    components: {
      [Table.name]: Table,
      [TableColumn.name]: TableColumn,
      [Dropdown.name]: Dropdown,
      [DropdownItem.name]: DropdownItem,
      [DropdownMenu.name]: DropdownMenu,
    },
    data() {
      return {
        tableData: []
      }
    },
    async mounted() {
        this.loadData();
    },
    methods: {
      loadData() {
        axios.get('reports/books-by-author').then(response => (this.tableData = response.data.data))
      },
      formatPrice(value) {
        return BRL.format(value);
      }
    }
  }
</script>
<style>
</style>

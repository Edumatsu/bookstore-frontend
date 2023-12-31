<template>
    <b-card no-body>
        <el-table class="table-responsive table"
                  header-row-class-name="thead-light"
                  :data="books">
            <el-table-column label="Título"
                             min-width="250px"
                             prop="title">
                <template v-slot="{row}">
                    <span class="font-weight-600 text-truncate mb-0 text-sm">{{row.title}}</span>
                </template>
            </el-table-column>
            <el-table-column label="Editora"
                             min-width="120px">
                <template v-slot="{row}">
                    <span class="text-truncate mb-0 text-sm">{{row.publishingCompany}}</span>
                </template>
            </el-table-column>
            <el-table-column label="Edição"
                             prop="edition"
                             min-width="120px">
            </el-table-column>

            <el-table-column label="Autores"
                             min-width="300px"
                             prop="authors">
                <template v-slot="{row}">
                    <span class="text-truncate mb-0 text-sm">{{row.authors.map(e=>e.name).join(', ')}}</span>
                </template>
            </el-table-column>

            <el-table-column label="Ações"
                             min-width="140px">
                <template v-slot="{row}">
                    <b-button size="sm" v-on:click="edit(row)" variant="transparent">
                        <i title="Editar" class=" mb-0 ni ni-ruler-pencil text-primary"></i> 
                    </b-button>
                    <b-button size="sm" v-on:click="confirmDelete(row)" variant="transparent">
                        <i title="Excluir" class=" mb-0 ni ni-fat-remove text-danger"></i> 
                    </b-button>
                </template>
            </el-table-column>
        </el-table>

        <b-modal id="form" :title="editingTitle">
            <div>
                <b-form @submit="onSubmit" @reset="onReset">
                    <b-form-group
                        id="input-group-1"
                        label="Email address:"
                        label-for="input-1"
                        description="We'll never share your email with anyone else."
                    >
                        <b-form-input
                        id="input-1"
                        v-model="form.email"
                        type="email"
                        placeholder="Enter email"
                        required
                        ></b-form-input>
                    </b-form-group>

                    <b-form-group id="input-group-2" label="Your Name:" label-for="input-2">
                        <b-form-input
                        id="input-2"
                        v-model="form.name"
                        placeholder="Enter name"
                        required
                        ></b-form-input>
                    </b-form-group>

                    <b-form-group id="input-group-3" label="Food:" label-for="input-3">
                        <b-form-select
                        id="input-3"
                        v-model="form.food"
                        :options="foods"
                        required
                        ></b-form-select>
                    </b-form-group>

                    <b-form-group id="input-group-4" v-slot="{ ariaDescribedby }">
                        <b-form-checkbox-group
                        v-model="form.checked"
                        id="checkboxes-4"
                        :aria-describedby="ariaDescribedby"
                        >
                        <b-form-checkbox value="me">Check me out</b-form-checkbox>
                        <b-form-checkbox value="that">Check that out</b-form-checkbox>
                        </b-form-checkbox-group>
                    </b-form-group>

                    <b-button type="submit" variant="primary">Submit</b-button>
                    <b-button type="reset" variant="danger">Reset</b-button>
                </b-form>
                <b-card class="mt-3" header="Form Data Result">
                <pre class="m-0">{{ form }}</pre>
                </b-card>
            </div>
        </b-modal>
    </b-card>
</template>
<script>
  import { Table, TableColumn} from 'element-ui'
  import axios from '../../../plugins/axios'
  export default {
    name: 'light-table',
    components: {
      [Table.name]: Table,
      [TableColumn.name]: TableColumn
    },
    data() {
      return {
        currentPage: 1,
        books: [],
        editingTitle: "Novo Livro",
        editingRow: {},
        form: {
          id: null,
          title: '',
          publishingCompany: '',
          edition: null,
          yearPublication: null,
          value: null,
          authors: [],
          subjects: []
        },
      };
    },
    async mounted() {
        axios
      .get('books')
      .then(response => (this.books = response.data.data))
        
    },
    methods: {
        onSubmit(event) {
            event.preventDefault()
            alert(JSON.stringify(this.form))
        },
        edit(row) {
            this.editingRow = row
            this.editingTitle = `Editando "${row.title}"`
            this.$bvModal.show("form")
        },
        confirmDelete(row) {
            this.$bvModal.msgBoxConfirm(`Deseja realmente remover o Livro "${row.title}."?`, {
                title: 'Tem certeza?',
                size: 'sm',
                buttonSize: 'sm',
                okVariant: 'danger',
                okTitle: 'Sim',
                cancelTitle: 'Não',
                footerClass: 'p-2',
                hideHeaderClose: true,
                centered: true
            })
            .then(value => {
                // deletar
            })
            .catch(err => {
                // An error occurred
            })
        }
    }
  }
</script>

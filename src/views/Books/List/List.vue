<template>
  <div>
    <b-alert :variant="alertVariant" :show="alertShow">{{alertText}}</b-alert>
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
                            min-width="140px" align="right">
                <template v-slot="{row}">
                    <b-button size="sm" v-on:click="edit(row)" variant="transparent">
                        <i title="Editar" class="mb-0 ni ni-ruler-pencil text-primary"></i> 
                    </b-button>
                    <b-button size="sm" v-on:click="confirmDelete(row)" variant="transparent">
                        <i title="Excluir" class="action-icon mb-0 ni ni-fat-remove text-danger"></i> 
                    </b-button>
                </template>
            </el-table-column>
        </el-table>

        <b-modal id="form" no-close-on-esc no-close-on-backdrop hide-footer :title="editingTitle">
            <div>
              <b-form @submit.stop.prevent="onSubmit">
                <b-form-group label="Título do Livro" label-for="title">
                  <b-form-input
                    id="title"
                    name="title"
                    v-model="$v.form.title.$model"
                    :state="validateState('title')"
                    aria-describedby="title-feedback"
                  ></b-form-input>

                  <b-form-invalid-feedback
                    id="title-feedback"
                  >
                    O título é obrigatório e pode conter no máximo 40 caracteres.
                  </b-form-invalid-feedback>
                </b-form-group>

                <b-form-group label="Editora" label-for="publishingCompany">
                  <b-form-input
                    id="publishingCompany"
                    name="publishingCompany"
                    v-model="$v.form.publishingCompany.$model"
                    :state="validateState('publishingCompany')"
                    aria-describedby="publishingCompany-feedback"
                  ></b-form-input>

                  <b-form-invalid-feedback
                    id="publishingCompany-feedback"
                  >
                    A Editora é obrigatória e pode conter no máximo 40 caracteres.
                  </b-form-invalid-feedback>
                </b-form-group>

                <div class="row">
                  <div class="col-md-4">
                    <b-form-group label="Edição" label-for="edition">
                      <b-form-input
                        id="edition"
                        name="edition"
                        type="number"
                        v-model="$v.form.edition.$model"
                        :state="validateState('edition')"
                        aria-describedby="edition-feedback"
                      ></b-form-input>
    
                      <b-form-invalid-feedback
                        id="edition-feedback"
                      >
                        A Edição é obrigatória e deve ser um número de no máximo 11 caracteres.
                      </b-form-invalid-feedback>
                    </b-form-group>
                  </div>
                  <div class="col-md-4">
                    <b-form-group label="Ano Publicação" label-for="yearPublication">
                      <b-form-input
                        id="yearPublication"
                        name="yearPublication"
                        type="number"
                        v-model="$v.form.yearPublication.$model"
                        :state="validateState('yearPublication')"
                        aria-describedby="yearPublication-feedback"
                      ></b-form-input>
    
                      <b-form-invalid-feedback
                        id="yearPublication-feedback"
                      >
                        O Ano de Publicação é obrigatório e deve conter 4 números.
                      </b-form-invalid-feedback>
                    </b-form-group>
                  </div>
                  <div class="col-md-4">
                    <b-form-group label="Valor" label-for="value"> 
                      <money 
                        id="value" 
                        name="value" 
                        class="form-control"
                        v-model="$v.form.value.$model" 
                        :state="validateState('value')" 
                        aria-describedby="value-feedback"></money> 
    
                      <b-form-invalid-feedback
                        id="value-feedback" 
                      >
                        A Editora é obrigatória e pode conter no máximo 40 caracteres.
                      </b-form-invalid-feedback>
                    </b-form-group>
                  </div>
                </div>

                <b-form-group 
                  label="Autores" 
                  label-for="authors"
                  description="Para selecionar mais de um autor, clique segurando o CONTROL">
                  <b-form-select
                    id="authors"
                    name="authors"
                    v-model="$v.form.authors.$model"
                    :options="authors"
                    :state="validateState('authors')"
                    multiple
                    aria-describedby="input-2-live-feedback"
                  ></b-form-select>
                  <b-form-invalid-feedback id="input-2-live-feedback">É necessário selecionar pelo menos um autor</b-form-invalid-feedback>
                </b-form-group>

                <b-form-group 
                  label="Assuntos" 
                  label-for="subjects"
                  description="Para selecionar mais de um assunto, clique segurando o CONTROL">
                  <b-form-select
                    id="subjects"
                    name="subjects"
                    v-model="$v.form.subjects.$model"
                    :options="subjects"
                    :state="validateState('subjects')"
                    multiple
                    aria-describedby="input-2-live-feedback"
                  ></b-form-select>
                  <b-form-invalid-feedback id="input-2-live-feedback">É necessário selecionar pelo menos um assunto</b-form-invalid-feedback>
                </b-form-group>

                <div class="row">
                  <div class="col-md-12 text-right">
                    <b-button type="button" v-on:click="hideModal()">Cancelar</b-button>
                    <b-button type="submit" variant="warning">Salvar</b-button>
                  </div>
                </div>
              </b-form>
            </div>
        </b-modal>
    </b-card>
  </div>
</template>
<script>
  import { Table, TableColumn} from 'element-ui'
  import axios from '../../../plugins/axios'
  import { validationMixin } from "vuelidate";
  import { required, minLength, maxLength } from "vuelidate/lib/validators";

  export default {
    name: 'light-table',
    mixins: [validationMixin],
    components: {
      [Table.name]: Table,
      [TableColumn.name]: TableColumn
    },
    data() {
      return {
        alertShow: false,
        alertVariant: "",
        alertText: "",
        currentPage: 1,
        books: [],
        authors: [],
        subjects: [],
        editingTitle: "Novo Livro",
        form: {
          id: null,
          title: '',
          publishingCompany: '',
          edition: null,
          yearPublication: null,
          value: 0, 
          authors: [],
          subjects: []
        },
      };
    },
    validations: {
      form: {
        title: {
          required,
          maxLength: maxLength(40)
        },
        publishingCompany: {
          required,
          maxLength: maxLength(40)
        },
        edition: {
          required,
          maxLength: maxLength(11)
        },
        yearPublication: {
          required,
          maxLength: maxLength(4),
          minLength: minLength(4),
        },
        value: { 
          required
        },
        authors: {
          required
        },
        subjects: {
          required
        }
      }
    },
    async mounted() {
        this.loadData();
    },
    methods: {
      loadData() {
        axios.get('books').then(response => (this.books = response.data.data))
      
        axios.get('authors').then(response => (
          this.authors = response.data.data.map( s => ({
            value: s.id,
            text: s.name,
          }))
        ))
        
        axios.get('subjects').then(response => (
          this.subjects = response.data.data.map( s => ({
            value: s.id,
            text: s.description,
          }))
        ))
      },
      validateState(name) {
        const { $dirty, $error } = this.$v.form[name];
        return $dirty ? !$error : null;
      },
      onSubmit() {
        this.$v.form.$touch();
        if (this.$v.form.$anyError) {
          return;
        }

        const form = this.form;
        form.authors = form.authors.map( s => ({id: s}))
        form.subjects = form.subjects.map( s => ({id: s}))

        const method = form.id > 0 ? 'put' : 'post'
        const path = form.id > 0 ? `books/${form.id}` : 'books'

        axios[method](path, form).then(response => {
          this.makeAlert('info', 'Informações registradas com sucesso!')
          this.$bvModal.hide("form")
          this.loadData()
        })

      },
      new() {
        this.form = {
          id: null,
          title: '',
          publishingCompany: '',
          edition: null,
          yearPublication: null,
          value: 0, 
          authors: [],
          subjects: []
        };

        this.$nextTick(() => {
          this.$v.$reset();
        });

        this.editingTitle = "Novo Livro";
        this.$bvModal.show('form');
      },
      edit(row) {
        row.authors = row.authors.map( s => (s.id))
        row.subjects = row.subjects.map( s => (s.id))
        this.form = row
        this.editingTitle = `Editando "${row.title}"`
        this.$bvModal.show("form")
      },
      hideModal() {
          this.form = {}
          this.$bvModal.hide("form")
      },
      makeAlert(variant, text) {
        this.alertVariant = variant;
        this.alertText = text;
        this.alertShow = true;

        setTimeout(() => this.alertShow = false, 5000);
      },
      confirmDelete(row) {
        this.$bvModal.msgBoxConfirm(`Deseja realmente remover o Livro "${row.title}"?`, {
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
          if (value) {
            axios.delete(`books/${row.id}`).then(response => {
              this.makeAlert('info', 'Livro excluído com sucesso!')
              this.loadData()
            })
          }
        })
        .catch(err => {
            // An error occurred
        })
      }
    }
  }
</script>
<style scoped>
.action-icon {
  font-size: 18px;
}
</style>
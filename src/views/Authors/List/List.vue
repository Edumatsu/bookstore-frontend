<template>
  <div>
    <b-alert :variant="alertVariant" :show="alertShow">{{alertText}}</b-alert>
    <b-card no-body>
        
        <el-table class="table-responsive table"
                  header-row-class-name="thead-light"
                  :data="authors">
            <el-table-column label="Nome"
                            prop="name">
                <template v-slot="{row}">
                    <span class="font-weight-600 text-truncate mb-0 text-sm">{{row.name}}</span>
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
                <b-form-group label="Nome" label-for="name">
                  <b-form-input
                    id="name"
                    name="name"
                    v-model="$v.form.name.$model"
                    :state="validateState('name')"
                    aria-describedby="name-feedback"
                  ></b-form-input>

                  <b-form-invalid-feedback
                    id="name-feedback"
                  >
                    O Nome do autor é obrigatório e pode conter no máximo 40 caracteres.
                  </b-form-invalid-feedback>
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
        authors: [],
        editingTitle: "Novo Autor",
        form: {
          id: null,
          name: '',
        },
      };
    },
    validations: {
      form: {
        name: {
          required,
          maxLength: maxLength(40)
        }
      }
    },
    async mounted() {
        this.loadData();
    },
    methods: {
      loadData() {
        axios.get('authors').then(response => (this.authors = response.data.data))
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
        const method = form.id > 0 ? 'put' : 'post'
        const path = form.id > 0 ? `authors/${form.id}` : 'authors'

        axios[method](path, form).then(response => {
          this.makeAlert('info', 'Informações registradas com sucesso!')
          this.$bvModal.hide("form")
          this.loadData()
        })

      },
      new() {
        this.form = {
          id: null,
          name: ''
        };

        this.$nextTick(() => {
          this.$v.$reset();
        });

        this.editingTitle = "Novo Autor";
        this.$bvModal.show('form');
      },
      edit(row) {
        this.form = row
        this.editingTitle = `Editando "${row.name}"`
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
        this.$bvModal.msgBoxConfirm(`Deseja realmente remover o Autor "${row.name}"?`, {
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
            axios.delete(`authors/${row.id}`).then(response => {
              this.makeAlert('info', 'Autor excluído com sucesso!')
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
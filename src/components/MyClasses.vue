<template>
    <div>
        <v-layout row wrap>
           <v-flex md12>
                <h3 class="title my-tests-title text-xs-center ma-1 pt-4">Minhas Turmas</h3>
            </v-flex>
            <v-flex  md4>
                <v-card class="green darken-1 white--text ma-5 text-xs-center">
                    <v-card-title primary-title>
                        <v-flex xs12>
                            <div class="headline">0</div>
                        </v-flex>
                        <v-flex xs12>
                            <div class="pt-3">Provas Aplicadas</div>
                        </v-flex>
                    </v-card-title>
                </v-card>
            </v-flex>
            <v-flex  md4>
                <v-card class="indigo lighten-1 white--text ma-5 text-xs-center">
                    <v-card-title primary-title>
                        <v-flex xs12>
                            <div class="headline">0</div>
                        </v-flex>
                        <v-flex xs12>
                            <div class="pt-3">Provas Aplicadas</div>
                        </v-flex>
                    </v-card-title>
                </v-card>
            </v-flex>
            <v-flex  md4>
                <v-card class="orange darken-1 white--text ma-5 text-xs-center">
                    <v-card-title primary-title>
                        <v-flex xs12>
                            <div class="headline">0</div>
                        </v-flex>
                        <v-flex xs12>
                            <div class="pt-3">Provas Aplicadas</div>
                        </v-flex>
                    </v-card-title>
                </v-card>
            </v-flex>
            <v-flex>
                <v-card transparent>
                     <v-btn
                        absolute
                        dark
                        buttom
                        fab
                        small
                        top
                        right
                        class="pink"
                        v-on:click="linkes"
                      >
              <v-icon>add</v-icon>
            </v-btn>
                </v-card>
            </v-flex>
            <v-flex md12 class="mr-5 ml-5 pa-1">
                <v-data-table
                    :headers="headers"
                    :items="classes"
                    :pagination.sync="pagination"
                    hide-actions
                    class="white elevation-1"
                >
                    <template slot="items" scope="props">
                        <td class="text-xs-center">{{ props.item.description }}</td>
                        <td class="text-xs-center">{{ props.item.size }}</td>
                        <td class="text-xs-center">
                          <v-btn class="red white--text darken-1" v-on:click="deleteClass(props.item.id)">Apagar</v-btn>
                        </td>
                    </template>
                </v-data-table>
                <div class="text-xs-center pt-2">
                    <v-pagination v-model="pagination.page" :length="pages"></v-pagination>
                </div>
            </v-flex>
        </v-layout>
    </div>
</template>

<script>
import baseService from '../services/baseService'
import auth from '../auth'

export default {
  name: 'myclasses',
  data () {
    return {
      pagination: {
        rowsPerPage: 3
      },
      headers: [
        {text: 'Descrição', value: 'description', align: 'center'},
        {text: 'Quantidade de alunos', value: 'size', align: 'center'},
        {text: 'Ações', value: '', align: 'center'}
      ],
      classes: []
    }
  },
  mounted () {
    this.getClasses()
  },
  methods: {
    getClasses () {
      baseService.get(`/teacher/${auth.teacherId()}/classes`).then(r => {
        if (r.status === 200) {
          this.classes = r.data.map(item => {
            return {
              description: item.description,
              id: item.id,
              size: item.size
            }
          })
        } else {
          this.$toastr('error', {position: 'toast-top-right', msg: 'Houve um erro na obtenção de turmas!'})
        }
      })
    },
    deleteClass (id) {
      baseService.del(`/class/${id}`).then(r => {
        this.getClasses()
      })
    },
    linkes () {
      this.$router.push('/turma')
    }
  },
  computed: {
    pages () {
      return this.pagination.rowsPerPage ? Math.ceil(this.classes.length / this.pagination.rowsPerPage) : 0
    }
  }
}
</script>

<style>

.title {
  color: #006;
}

</style>

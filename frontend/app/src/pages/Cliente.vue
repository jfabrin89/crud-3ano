<template>
  <div id="cliente">
    <Cabecalho />
    <v-content class="ma-3">
      <h1>Cliente</h1>
      <hr />
      <v-layout wrap>
        <v-flex xs12>
          <v-text-field v-model="filtro" label="Filtrar"></v-text-field>
        </v-flex>
      </v-layout>
      <v-data-table :headers="tblCliente.cabecalho" :items="tblCliente.item" hide-actions class="elevation-1" :search="filtro">
        <template slot="items" slot-scope="props">
          <td>{{ props.item.id }}</td>
          <td>{{ props.item.nome }}</td>
          <td>{{ props.item.telefone }}</td>
          <td>{{ props.item.email }}</td>
          <td>{{ props.item.situacao | situacao }}</td>
          <td class="justify-center">
            <v-btn small fab flat @click="editarCliente(props.item)">
              <v-icon>edit</v-icon>
            </v-btn>
            <v-btn small fab flat @click="modal.mensagem = true, teste = props.item">
              <v-icon>delete</v-icon>
            </v-btn>
          </td>
        </template>
        <template slot="no-data">
          <v-btn flat color="primary" @click="buscarDados">
            <v-icon class="mr-2">autorenew</v-icon>
            Recarregar
          </v-btn>
        </template>
      </v-data-table>
      <div style="position: relative">
        <v-btn absolute small dark fab top right @click="adicionarRegistro">
          <v-icon>add</v-icon>
        </v-btn>
      </div>
    </v-content>

    <ModalClienteCadastro :modal="modal.cliente" :registro="cliente" v-on:cliente="closeCliente"/>

    <!-- ModalMensagem -->
    <v-dialog v-model="modal.mensagem" max-width="290">
      <v-card>
        <v-card-title class="headline">Atenção!</v-card-title>

        <v-card-text>
          Deseja realmente excluir este registro?
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="red darken-1" flat="flat" @click="modal.mensagem = false">
            Cancelar
          </v-btn>

          <v-btn color="green darken-1" flat="flat" @click="excluirCliente(teste), modal.mensagem = false">
            Sim
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import Cabecalho from '@/components/Cabecalho'
import ModalClienteCadastro from '@/components/ModalClienteCadastro'
export default {
  components: {
    Cabecalho,
    ModalClienteCadastro
  },
  created () {
    this.buscarDados()
  },
  data () {
    return {
      cliente: '',
      filtro: '',
      modal: {
        cliente: false,
        mensagem: false
      },
      teste: '',
      tblCliente: {
        cabecalho: [
          {text: '#', value: 'id'},
          {text: 'Nome', value: 'nome', align: 'left'},
          {text: 'Telefone', value: 'telefone', align: 'left'},
          {text: 'E-Mail', value: 'email', align: 'left'},
          {text: 'Situação', value: 'situacao'},
          {text: 'Opções', value: ''}
        ],
        item: []
      }
    }
  },
  filters: {
    situacao (dados) {
      if (dados === 'A') return 'Ativo'
      else return 'Inativo'
    }
  },
  methods: {
    adicionarRegistro () {
      this.cliente = ''
      this.modal.cliente = true
    },
    buscarDados () {
      this
        .axios
        .get('clientes')
        .then((success) => {
          this.tblCliente.item = success.data
        })
        .catch((error) => {
          console.log(error)
        })
    },
    closeCliente (val) {
      this.buscarDados()
      this.modal.cliente = val
    },
    editarCliente (cliente) {
      this.cliente = cliente
      this.modal.cliente = true
    },
    excluirCliente (item) {
      this
        .axios
        .delete('clientes/' + item.id)
        .then((success) => {
          this.mensagem = {
            mostrar: true,
            texto: 'Excluído com sucesso!',
            tipo: 'success'
          }
          this.buscarDados()
        })
        .catch((error) => {
          this.mensagem = {
            mostrar: true,
            texto: 'Erro ao excluir' + error,
            tipo: 'error'
          }
        })
    }
  },
  name: 'cliente'
}
</script>

<style scoped>

</style>

<template>
  <div id="modalClienteCadastro">
    <v-dialog v-model="modal" fullscreen hide-overlay transition="dialog-bottom-transition">
      <v-card>
        <v-toolbar dark>
          <v-btn icon dark @click.native="close(false)">
            <v-icon>close</v-icon>
          </v-btn>
          <v-toolbar-title>Cadastro de Cliente</v-toolbar-title>
        </v-toolbar>
        <v-layout row wrap justify-center>
          <v-flex xs8>
            <v-alert :type="mensagem.tipo" :value="true" v-if="mensagem.mostrar">
              {{mensagem.texto}}
            </v-alert>

            <v-form v-model="valid" ref="form" lazy-validation>
              <v-text-field v-model="cliente.id" label="ID" disabled></v-text-field>
              <v-text-field v-model="cliente.nome" label="Nome" :rules="regrasValidacao.nome" required></v-text-field>
              <v-text-field v-model="cliente.telefone" label="Telefone" :mask="mascara.celular || mascara.fixo" return-masked-value></v-text-field>
              <v-text-field v-model="cliente.email" label="E-Mail" :rules="regrasValidacao.email" required></v-text-field>
              <v-select v-model="cliente.situacao" label="Situação" :items="cbb.situacao" :rules="regrasValidacao.situacao" required></v-select>

              <v-btn outline color="success" :disabled="!valid" @click="salvar">Salvar</v-btn>
              <v-btn outline color="error" @click="limpar">Limpar</v-btn>
            </v-form>
          </v-flex>
        </v-layout>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data () {
    return {
      cliente: {
        nome: '',
        telefone: '',
        email: '',
        situacao: 'A'
      },
      cbb: {
        situacao: [
          {text: 'Ativo', value: 'A'},
          {text: 'Inativo', value: 'I'}
        ]
      },
      mascara: {
        celular: '(##)#####-####',
        fixo: '(##)####-####'
      },
      mensagem: {
        mostrar: false,
        tipo: '',
        texto: ''
      },
      regrasValidacao: {
        nome: [
          v => !!v || 'Nome é obrigatório'
        ],
        email: [
          v => !!v || 'E-mail é obrigatório',
          v => /.+@.+/.test(v) || 'E-mail não é válido'
        ],
        situacao: [
          v => !!v || 'Situação é obrigatório'
        ]
      },
      valid: true
    }
  },
  methods: {
    close (fechar) {
      this.$emit('cliente', fechar)
    },
    limpar () {
      this.cliente = {
        nome: '',
        telefone: '',
        email: '',
        situacao: 'A'
      }
      this.form = Object.assign({}, this.defaultForm)
      this.$refs.form.reset()
    },
    salvar () {
      if (this.$refs.form.validate()) {
        if (!this.cliente.id) {
          this
            .axios
            .post('clientes', this.cliente)
            .then((success) => {
              this.mensagem = {
                mostrar: true,
                texto: 'Salvo com sucesso!',
                tipo: 'success'
              }
              this.limpar()
            })
            .catch((error) => {
              this.mensagem = {
                mostrar: true,
                texto: 'Erro ao salvar' + error,
                tipo: 'error'
              }
            })
        } else {
          this
            .axios
            .put('clientes/' + this.cliente.id, this.cliente)
            .then((success) => {
              this.mensagem = {
                mostrar: true,
                texto: 'Alterado com sucesso!',
                tipo: 'success'
              }
              this.limpar()
            })
            .catch((error) => {
              this.mensagem = {
                mostrar: true,
                texto: 'Erro ao salvar' + error,
                tipo: 'error'
              }
            })
        }
      }
    }
  },
  props: [
    'modal',
    'registro'
  ],
  watch: {
    registro: function (val) {
      if (val !== '') this.cliente = val
    }
  }
}
</script>

<style>

</style>

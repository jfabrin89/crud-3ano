<template>
  <div id="modalMensagem">
    <v-dialog v-model="modal.mensagem" max-width="290">
      <v-card>
        <v-card-title class="headline">Atenção!</v-card-title>

        <v-card-text>
          Deseja realmente excluir este registro?
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="red darken-1" flat="flat" @click="dialog = false">
            Cancelar
          </v-btn>

          <v-btn color="green darken-1" flat="flat" @click="dialog = false">
            Sim
          </v-btn>
        </v-card-actions>
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

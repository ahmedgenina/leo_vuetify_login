<template>
  <v-app>
    <v-app-bar app dense class="primary white--text pl-0 pl-md-10 pl-lg-16">
      <v-btn depressed small class="ml-0 primary white--text" v-if="!usuarioEstaLogado" 
      :title="mensagemTrocaAcaoForm" @click="trocaAcaoForm">{{ mensagemTrocaAcaoForm }}</v-btn>
      <v-btn class="error lighten-1 white--text" v-if="usuarioEstaLogado" @click="sair">Sair</v-btn>
    </v-app-bar>
      
      <v-main app>
        <v-container fill-height>
          <v-layout row wrap justify-center>
            <v-flex xs11 sm6 class="pt-xs-2 pt-sm-8 pt-md-16 ">
              <FormUsuario v-if="!usuarioEstaLogado" :acaoForm="acaoForm" @registrar.self="registrarUsuario" 
              @logar.self="logarUsuario"></FormUsuario>
              <h2 v-else class="text-center">{{ mensagemUsuarioLogado }}</h2>
            </v-flex>
          </v-layout>
        </v-container>
      </v-main>
    
  </v-app>
</template>

<script>
  import Vue from 'vue'
  import Vuetify from 'vuetify/lib';
  import FormUsuario from './components/FormUsuario/FormUsuario'
  
  Vue.use(Vuetify);
  
  export default {
    

    name: 'App',

    components: {
      FormUsuario
    },

    data: function(){
      return {
        usuariosRegistrados: [],
        usuarioEstaLogado: false,
        nomeUsuarioLogado: '',
        acaoForm: 'registrar',
        mensagemErro: ''
      }
    },

    computed: {
      mensagemTrocaAcaoForm: function(){
        if(this.acaoForm === 'registrar'){
          return 'Já estou registrado'
        } else {
          return 'Registrar-se agora'
        }
      },
      mensagemUsuarioLogado: function(){
        return 'Bem-vindo(a) ' + this.nomeUsuarioLogado + '!'
      }
    },

    methods: {
      registrarUsuario: function(novoUsuario){
        this.mensagemErro = ''

        let buscaUsuario = this.usuariosRegistrados.filter(obj => {
          return obj.nomeDeUsuario === novoUsuario.nomeDeUsuario
        })

        if(buscaUsuario.length === 0){
          /* É criado um novo objeto para evitar que
          mudanças no FormUsuario alterem o valor do objeto em
          usuariosRegistrados (isso acontece pois Array.push faz
          uma cópia rasa do objeto)
          
          https://stackoverflow.com/a/8661088
          */
          let _novoUsuario = {
            nomeDeUsuario: novoUsuario.nomeDeUsuario,
            senha: novoUsuario.senha
          }
          this.usuariosRegistrados.push(_novoUsuario)
        } else {
          this.mensagemErro = 'Esse usuário já existe'
        }
        
        this.trocaAcaoForm()
      },
      logarUsuario: function(usuario){
        this.mensagemErro = ''
        let buscaUsuario = this.usuariosRegistrados.filter(obj => {
          return obj.nomeDeUsuario === usuario.nomeDeUsuario
        })

        if (buscaUsuario.length > 0 && usuario.senha === buscaUsuario[0].senha){
          this.nomeUsuarioLogado = usuario.nomeDeUsuario
          this.usuarioEstaLogado = true
        } else {
          this.mensagemErro = 'Este usuário não existe ou a senha está incorreta'
        }
      },
      sair: function(){
        this.mensagemErro = ''
        this.nomeUsuarioLogado = ''
        this.usuarioEstaLogado = false
      },
      trocaAcaoForm: function(){
        this.mensagemErro = ''
        if(this.acaoForm === 'registrar'){
          this.acaoForm = 'logar'
        } else {
          this.acaoForm = 'registrar'
        }
      }
    }
  };
</script>

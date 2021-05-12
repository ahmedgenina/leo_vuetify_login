<template>
    <v-card class="mx-auto" min-height="45%">
        <v-card-text>
            <v-container class="text-center">
                <v-form>
                    <v-flex class="text-left">
                        <div class="text-h5">{{ textoBotao }}</div>
                    </v-flex>
                    <v-flex xs12>
                        <v-text-field v-model="usuario.nomeDeUsuario" name="nomeDeUsuario" label="Nome de usuário" title="Nome de usuário"
                        placeholder="Nome de usuário"></v-text-field>
                    </v-flex>
                    <v-flex xs12>
                        <v-text-field v-model="usuario.senha" type="password" name="senha" label="Senha" title="Senha"
                        placeholder="Senha"></v-text-field>
                    </v-flex>
                    <v-flex xs12>
                        <v-btn @click.prevent="$emit(acaoForm, usuario)" class="primary" :title="textoBotao">{{ textoBotao }}</v-btn>
                    </v-flex>
                </v-form>
            </v-container>
        </v-card-text>
    </v-card>
</template>

<script>
export default {
    name: 'FormUsuario',
    props: {'acaoForm': String},
    components: {},

    data: function(){
        return {
            usuario: {
                nomeDeUsuario: '',
                senha: ''
            }
        }
    },

    computed: {
        textoBotao: function(){
            if (this.acaoForm === 'registrar'){
                return 'Registrar'
            } else {
                return 'Entrar'
            }
        }
    }
}
</script>
<template>
<div class="container" >

    <h2> Cadastro de Autores</h2>
    <b-form style="width: 80vw">
      <b-form-group

      >
        <b-form-input
          id="input-1"
          v-model="form.titulo"
          type="email"
          placeholder="Titulo"
          required
        ></b-form-input>
      </b-form-group>

      <b-form-group>
        <b-form-input
          id="input-2"
          v-model="form.ano_lancamento"
          placeholder="Ano de lançamento"
          required
        ></b-form-input>
      </b-form-group>
        
        <b-form-group >
        <b-form-input
          id="input-2"
          v-model="form.editora"
          placeholder="Editora"
          required
        ></b-form-input>
        </b-form-group>

        <b-form-group>
        <b-form-input
          id="input-2"
          v-model="form.genero"
          placeholder="Genero"
          required
        ></b-form-input>
        </b-form-group>

        <b-form-group >
        <b-form-input
          id="input-2"
          v-model="form.autor"
          placeholder="Autor"
          required
        ></b-form-input>
      </b-form-group>

      <center>      <b-button v-if="button" type="submit" variant="primary"   @click.prevent="createLivros">Cadastrar</b-button>
      <b-button v-if="!button" type="submit" class="" variant="primary" @click.prevent="updateLivros">Atualizar</b-button>
</center>

        </b-form>
        <b-card class="mt-3" style="width: 80vw" header="Lista de Livros">
      <table v-for="(livro, index) in livros" :key="index">
        <thead>
          <tr>
              <th style="min-width:150px">Titulo</th>
              <th style="min-width:30px">Ano de lançamento</th>
              <th style="min-width:150px">Editora</th>
              <th style="min-width:150px">Genero</th>
              <th style="min-width:150px">Autor</th>
              
              <th >Ação</th>
          </tr>
        </thead>

        <tbody>
          <tr>
            <td>{{livro.nome}}</td>
            <td>{{livro.ano_lancamento}}</td>
            <td>{{livro.editora}}</td>
            <td>{{livro.genero}}</td>
            <td>{{livro.autor}}</td>
            <td>    
              <b-button-group>
              <b-button @click="editar(livro)" variant="success" style="margin-right:10px">Editar</b-button>
              <b-button @click="deleteGeneros(livro.id)" variant="danger">Excluir</b-button>

  
    </b-button-group></td>
 </tr>
  
        </tbody>
      </table>
            
    </b-card>
  </div>
</template>
<script>
import apiLib from "../../services/config"


export default {

  data(){
 return {
       form:{
           nome:"",
           data_nasc: null,
           sexo: "",
           nacionalidade:""
          },

        button: true,
        idAutor: "",
        autores:[],
     }
  },
mounted(){
  this.getAutores();
},
 methods:{
   createAutor(){
     apiLib
      .createAutor(this.form)
      .then (({data})=> {
        console.log(data)
        this.form ={
                nome: "",
                data_nasc: null,
                sexo: "",
                nacionalidade: ""
        };
        this.getAutores();
      })
      .catch((err) => {
        console.log(err)
      })
   },

   getAutores() {
     apiLib
     .getAutores()
      .then (({data})=> {
          this.autores = data

      })
     },

    deleteAutor(id){
        apiLib
        .deleteAutor(id)
        .then(()=> {
            this.getAutores()
        })
    },
    editar(autor){
        this.idAutor = autor.id,
        this.form.nome = autor.nome,
        this.form.data_nasc = autor.data_nasc,
        this.form.sexo = autor.sexo,
        this.form.nacionalidade = autor.nacionalidade

        this.button = false
    },
    updateAutor(){
        apiLib
        .updateAutor(this.idAutor, this.form)
        .then(()=> {
                    this.form ={
                nome: "",
                data_nasc: null,
                sexo: "",
                nacionalidade: ""
        };
        this.getAutores();
        this.button = true
        })

    }
   }

}
</script>


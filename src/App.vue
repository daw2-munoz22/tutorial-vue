<!--
  La estructura es la siguiente:
    <template>
      Aqui incluye el codigo HTML
    </template>
    
    <script>
      Aqui incluye todo el JavaScript
    export default {
      Sirve para expertar todo el componente, con sus propiedades y métodos
    }
    </script>
    
    <style>
      Aqui incluye el CSS
    </style>
-->
<template>
  <div id="app" class="container">
    <div class="row">
      <div class="col-md-12">
        <h1>Personas</h1>
      </div>
    </div>
    <div class="row">
      <div class="col-md-12">
        <FormularioPersona  @add-persona="agregarPersona"/> <!-- Con @add-persona capturamos los datos en la aplicación-->
        <TablaPersonas :personas="personas" @delete-persona="eliminarPersona" @actualizar-persona="actualizarPersona" /><!--Inyecto el componente TablaPersonas--><!--Con v-bind puedo agregar propiedades al componente-->
      </div>
    </div>
  </div>
</template>

<script>
  import TablaPersonas from './components/TablaPersonas.vue' //Para poder utilizar el componente TablaPersonas, debo importar dicho componente. Para ello, pongo la ruta donde está guardado el archivo que incluye ese componente
  import FormularioPersona from './components/FormularioPersona.vue'
  
  export default {
    name: 'app', //Es el nombre del componente donde estoy situado
    components: { //Aqui se añadiran todos los componentes
      TablaPersonas,
      FormularioPersona,
    },
    //agregar el método data, que devolvera los datos de los usuarios
    data(){
      return {
        // la propiedad personas es un array de objetos
        personas: [
          {
            id: 1,
            nombre: 'Jon',
            apellido: 'Nieve',
            email: 'jon@email.com'
          },
          {
            id: 2,
            nombre: 'Tyrion',
            apellido: 'Lannister',
            email: 'tyrion@email.com',
          },
          {
            id: 3,
            nombre: 'Daenerys',
            apellido: 'Targaryen',
            email: 'daenerys@email.com',
          },
        ],
      }
    },
    methods: {
      agregarPersona(persona){ /*Este metodo recibe el objeto persona y se irá modificando ese array*/ 
      // ... sirve para combinar objetos y arrays, para crear un nuevo array que contenga los elemento antiguos del array personas junto con la nueva.
        let id = 0;
        /*por ahora nos limitaremos a generar un ID basándonos en el ID del elemento inmediatamente anterior al actual */
        /* Los que hemos hecho es aumentar el valor del ID del último elemento agregado en una unidad, o dejarlo en 0 si no hay elementos. Luego insertamos la persona en el array, a la que agregamos el id generado.*/
        if (this.personas.length > 0) {
          id = this.personas[this.personas.length - 1].id + 1;
        }
        this.personas = [...this.personas, { ...persona, id}];
      },
      eliminarPersona(id) {
        this.personas = this.personas.filter(
            persona => persona.id !== id
        );
      },
      actualizarPersona(id, personaActualizada) {
        this.personas = this.personas.map(persona =>
          persona.id === id ? personaActualizada : persona
        )
      }
    },
  }
</script>

<style>
  button {
    background: #009435;
    border: 1px solid #009435;
  }
</style>

<template>
  <div id="tabla-personas">
    <div v-if="!personas.length" class="alert alert-info" role="alert">
        No se han agregado personas
    </div>
    <table class="table">
      <thead>
        <tr>
          <th>Nombre</th>
          <th>Apellido</th>
          <th>Email</th>
        </tr>
      </thead>
      <tbody>
        <!--El v-for es equivalente a ForEach. Recorro el array personas, y en la variable persona guardo todas las propiedades del array. Es obligatorio tener una key, que en este caso, es un ID único-->
        <tr v-for="persona in personas" :key="persona.id">
          <td v-if="editando === persona.id">
            <input type="text" class="form-control" v-model="persona.nombre" />
          </td>
          <td v-else>
            {{ persona.nombre}}
          </td>
          <td v-if="editando === persona.id">
            <input type="text" class="form-control" v-model="persona.apellido" />
          </td>
          <td v-else>
            {{ persona.apellido}}
          </td>
          <td v-if="editando === persona.id">
            <input type="email" class="form-control" v-model="persona.email" />
          </td>
          <td v-else>
            {{ persona.email}}
          </td>
          <td v-if="editando === persona.id">
            <button class="btn btn-success" @click="guardarPersona(persona)">💾 Guardar</button>
            <button class="btn btn-secondary ml-2" @click="cancelarEdicion(persona)">❌ Cancelar</button>
          </td>
          <td v-else>
            <button class="btn btn-info" @click="editarPersona(persona)">✏️ Editar</button>
            <button class="btn btn-danger ml-2" @click="$emit('delete-persona', persona.id)">🗑️ Eliminar</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  export default {
    name: 'tabla-personas', //nombre del componente
    //El objeto props debe contener todas aquellas propiedades que va a recibir el componente, conteniendo pares que contienen el nombre de la propiedad y el tipo de las misma
    props: {
        personas: Array, //Para declarar que la propiedad personas es un array, se declara Array, NO []
    },
    data() {
        return {
            editando: null,
        }
    },
    methods: {
      editarPersona(persona) {
        this.personaEditada = Object.assign({}, persona);
        this.editando = persona.id;
      },
      guardarPersona(persona) {
        if (!persona.nombre.length || !persona.apellido.length || !persona.email.length) {
          return;  
        }
        this.$emit('actualizar-persona', persona.id, persona);
        this.editando = null;
      },
      cancelarEdicion(persona) {
        Object.assign(persona, this.personaEditada);
        this.editando = null;
      }
    }
  }
</script>

<style scoped></style>
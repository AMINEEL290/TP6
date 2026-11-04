<template>
  <div v-if="student">
    <h1>{{ student.firstName }} {{ student.lastName }}</h1>

    <div>
      <label>Prénom</label>
      <input v-model="student.firstName" type="text" />
    </div>
    <div>
      <label>Nom</label>
      <input v-model="student.lastName" type="text" />
    </div>
    <div>
      <label>Filière</label>
      <input v-model="student.filiere" type="text" />
    </div>
    <div>
      <label>Année</label>
      <input v-model.number="student.year" type="number" min="1" max="5" />
    </div>
    <div>
      <label>Email</label>
      <input v-model="student.email" type="email" />
    </div>
  </div>
  <div v-else>
    <p>Étudiant introuvable.</p>
  </div>

  <button @click="$router.back()">Retour</button>
</template>

<script>
import { store } from '../store/students.js'

export default {
  name: 'StudentDetailView',
  computed: {
    student() {
      const id = Number(this.$route.params.id)
      return this.store.students.find(student => student.id === id) || null
    }
  },
  data() {
    return {
      store
    }
  }
}
</script>

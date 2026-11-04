<template>
  <h1>Annuaire des étudiants</h1>

  <form @submit.prevent="ajouterEtudiant">
    <input v-model="newStudent.firstName" type="text" placeholder="Prénom" />
    <input v-model="newStudent.lastName" type="text" placeholder="Nom" />
    <input v-model="newStudent.filiere" type="text" placeholder="Filière" />
    <input v-model.number="newStudent.year" type="number" min="1" max="5" placeholder="Année" />
    <input v-model="newStudent.email" type="email" placeholder="Email" />
    <button type="submit">Ajouter</button>
  </form>

  <div class="toolbar">
    <input v-model="recherche" type="text" placeholder="Rechercher un étudiant" />
    <select v-model="tri">
      <option value="lastName">Nom</option>
      <option value="filiere">Filière</option>
    </select>
  </div>

  <StudentCard v-for="student in etudiantsAffiches" :key="student.id" :student="student" />
</template>

<script>
import StudentCard from '../components/StudentCard.vue'
import { store } from '../store/students.js'

export default {
  name: 'StudentListView',
  components: { StudentCard },
  data() {
    return {
      store,
      recherche: '',
      tri: 'lastName',
      newStudent: { firstName: '', lastName: '', filiere: '', year: 1, email: '' }
    }
  },
  computed: {
    etudiantsAffiches() {
      const terme = this.recherche.trim().toLowerCase()
      let result = [...this.store.students]

      if (terme) {
        result = result.filter(student => {
          const nom = student.lastName.toLowerCase()
          const prenom = student.firstName.toLowerCase()
          return nom.includes(terme) || prenom.includes(terme)
        })
      }

      result.sort((a, b) => {
        if (this.tri === 'filiere') {
          return a.filiere.localeCompare(b.filiere)
        }
        return a.lastName.localeCompare(b.lastName)
      })

      return result
    }
  },
  methods: {
    ajouterEtudiant() {
      const firstName = this.newStudent.firstName.trim()
      const lastName = this.newStudent.lastName.trim()

      if (!firstName || !lastName) {
        return
      }

      const etudiant = {
        id: Date.now(),
        firstName,
        lastName,
        filiere: this.newStudent.filiere.trim() || 'Non précisée',
        year: Number(this.newStudent.year) || 1,
        email: this.newStudent.email.trim()
      }

      this.store.students.push(etudiant)
      this.newStudent = { firstName: '', lastName: '', filiere: '', year: 1, email: '' }
    }
  }
}
</script>

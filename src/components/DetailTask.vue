<template>
    <div class="task-details" style="width: 40%; padding: 10px;">
      <div v-if="taskDeleted">
        <p>La tâche "{{ deletedTaskName }}" a été supprimée récemment.</p>
      </div>
      <div v-else>
        <h1>Description de {{ task.text }}</h1>
        <p><strong>Détails :</strong> {{ task.details }}</p>
      </div>
    </div>
  </template>
  
  <script setup>
  import { defineProps } from 'vue';
  
  const props = defineProps({
    task: Object,
    taskDeleted: Boolean,
    deletedTaskName: String,
  });
  </script>
  

<style scoped>

.container {
  display: flex;
  justify-content: space-between; /* Espace entre les deux colonnes */
  padding: 20px; /* Espacement autour du container */
  background-color: #f8f9fa; /* Couleur de fond */
  min-height: 100vh; /* Hauteur minimale */
}

.todo-list {
  flex: 1; /* Prend tout l'espace disponible */
  margin-right: 20px; /* Espacement à droite */
}

.task-details {
  width: 70%; /* Largeur de la section des détails */
  max-width: 500px; /* Largeur maximale pour éviter qu'elle ne soit trop large */
  background-color: #ffffff; /* Couleur de fond blanche */
  border: 1px solid #dee2e6; /* Bordure grise */
  border-radius: 5px; /* Arrondi des coins */
  padding: 20px; /* Espacement interne */
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1); /* Ombre légère */ 
  position: fixed; /* Position fixe */
  top: 9px; /* Distance depuis le haut de la fenêtre */
  right: 20px; /* Aligné à droite */
  overflow: auto; /* Ajoute un défilement si le contenu dépasse la taille */
  white-space: pre-wrap; /* Préserve les espaces et les retours à la ligne */
}

.task-details h1 {
  font-size: 1.5rem; /* Taille du titre */
  margin-bottom: 15px; /* Espacement en bas */
  color: #343a40; /* Couleur du texte */
}

.task-details p {
  font-size: 1rem; /* Taille du texte */
  color: #495057; /* Couleur du texte secondaire */
  line-height: 1.5; /* Hauteur de ligne pour lisibilité */
}
</style>

<template>
    <div style="width: 70%; padding: 20px;">
      <div class="add-task-form">
        <input v-model="newTask.text" placeholder="Ajouter une nouvelle tâche" />
        <textarea v-model="newTask.details" placeholder="Détails de la tâche"></textarea>
        <input type="file" @change="onFileChange" accept="image/*" /> <!-- Champ pour télécharger une image -->
        <button @click="addTask">Ajouter Tâche</button>
      </div>
      <br>
      <div class="task-list">
        <h1>Liste des Tâches</h1>
        <div class="task-cards">
          <div v-if="tasks.length === 0" class="no-tasks">
            <img class="task-image" src="@/assets/koala.png" alt="Image" />
          </div>
          <div class="task-card" v-for="(task, index) in tasks" :key="index">
            <img v-if="task.image" class="task-image" :src="task.image" alt="Image de tâche" /> <!-- Afficher l'image -->
            <div class="task-content">
              <span class="task-status" :class="{ 'completed': task.completed }">
                {{ task.completed ? 'Complète' : 'Planifiée' }}
              </span>
              <h3 class="task-title" @click="selectTask(task)">
                {{ task.text }}
              </h3>
              <p class="task-description">{{ truncate(task.details, 15) }}</p>
              <div class="task-actions">
                <button @click="toggleTask(index)" class="btn"><i class="fas fa-check"></i></button>
                <button @click="removeTask(index)" class="btn"><i class="fas fa-trash"></i></button>
              </div>
            </div>
            <br>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, defineEmits } from 'vue';
  
  const emit = defineEmits(['task-selected', 'task-removed']);
  
  const newTask = ref({
    text: '',
    details: '',
    image: '' 
  });
  const tasks = ref([]);
  
  const addTask = () => {
    if (newTask.value.text.trim() === '') return;
    tasks.value.push({ 
      text: newTask.value.text, 
      details: newTask.value.details, 
      image: newTask.value.image, 
      completed: false 
    });
    newTask.value.text = '';
    newTask.value.details = '';
    newTask.value.image = ''; 
  };
  
  const toggleTask = (index) => {
    tasks.value[index].completed = !tasks.value[index].completed;
  };
  
  const removeTask = (index) => {
    const removedTask = tasks.value[index]; 
    tasks.value.splice(index, 1);
    emit('task-removed', removedTask); 
  };
  
  const selectTask = (task) => {
    emit('task-selected', task); 
  };
  
  const truncate = (str, length) => {
    return str.length > length ? str.substring(0, length) + '...' : str;
  };
  
  const onFileChange = (event) => {
    const file = event.target.files[0];
    if (file) {
      const reader = new FileReader();
      reader.onload = e => {
        newTask.value.image = e.target.result; 
      };
      reader.readAsDataURL(file); 
    }
  };
  </script>
  
<style scoped>
  
  .task-list {
      background-color: #f9f9f9;
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 20px;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
      color: #333;
      font-size: 1.5em;
      margin-bottom: 15px;
  }
  
  input,
  textarea {
      width: 100%; /* Largeur complète */
      padding: 10px; /* Espacement interne */
      margin: 10px 0; /* Espacement vertical */
      border: 1px solid #ccc; /* Bordure grise */
      border-radius: 4px; /* Coins arrondis */
      font-size: 1em; /* Taille de la police */
      box-sizing: border-box; /* Inclure padding et bordure dans la largeur totale */
  }
  
  input:focus,
  textarea:focus {
      border-color: #007BFF; /* Bordure bleue au focus */
      outline: none; /* Supprimer l'outline par défaut */
  }
  
  button {
      background-color: #007BFF;
      color: #fff;
      border: none;
      border-radius: 4px;
      padding: 10px 15px;
      cursor: pointer;
      font-size: 1em;
      margin-top: 10px;
  }
  
  button:hover {
      background-color: #0056b3;
  }
  
  .task-list {
      background-color: #f9f9f9;
      border: 1px solid #ddd;
      border-radius: 8px;
      padding: 20px;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
  }
  
  h1 {
      color: #333;
      font-size: 1.5em;
      margin-bottom: 15px;
  }
  
  input[type="text"],
  textarea {
      width: 100%;
      padding: 10px;
      margin: 10px 0;
      border: 1px solid #ccc;
      border-radius: 4px;
  }
  
  button {
      background-color: #007BFF;
      color: #fff;
      border: none;
      border-radius: 4px;
      padding: 10px 15px;
      cursor: pointer;
      font-size: 1em;
      margin-top: 10px;
  }
  
  button:hover {
      background-color: #0056b3;
  }
  
  .task-cards {
      display: flex;
      flex-wrap: wrap; /* Permet aux cartes de passer à la ligne si nécessaire */
      gap: 10px; /* Espacement entre les cartes */
  }
  
  .task-card {
      display: flex; /* Flex pour la disposition horizontale */
      background-color: #fff; /* Couleur de fond de la carte */
      border: 1px solid #dee2e6; /* Bordure grise */
      border-radius: 0.5rem; /* Arrondi des coins */
      box-shadow: 0 0.125rem 0.25rem rgba(0, 0, 0, 0.1); /* Ombre légère */
      width: calc(100% - 20px); /* Largeur des cartes */
      padding: 15px; /* Espacement interne */
      transition: box-shadow 0.2s; /* Transition pour l'ombre */
  }
  
  .task-card:hover {
      box-shadow: 0 0.5rem 1rem rgba(0, 0, 0, 0.2); /* Ombre plus forte au survol */
  }
  
  .task-image {
      width: 80px; /* Largeur fixe pour l'image */
      height: 80px; /* Hauteur automatique */
      border-radius: 0.5rem; /* Arrondi des coins de l'image */
      margin-right: 15px; /* Espacement à droite de l'image */
  }
  
  .task-content {
      display: flex;
      flex-direction: column; /* Aligner le contenu à la verticale */
      flex-grow: 1; /* Prendre tout l'espace restant */
  }
  
  .task-status {
      display: inline-block; /* Affichage en ligne */
      padding: 5px 10px; /* Espacement interne */
      border-radius: 0.25rem; /* Arrondi des coins */
      color: #fff; /* Couleur du texte */
      margin-bottom: 10px; /* Espacement en bas */
      background-color: #dba33a; /* Jert pour les tâches complètes */
  
  }
  
  .completed {
      background-color: #28a745; /* Vert pour les tâches complètes */
  }
  
  .task-title {
      font-size: 1.2rem; /* Taille du titre */
      color: #212529; /* Couleur du texte */
      cursor: pointer; /* Curseur indicatif sur le titre */
      margin: 0; /* Pas de marge */
  }
  
  .task-description {
      color: #6c757d; /* Couleur du texte secondaire */
      margin: 10px 0; /* Espacement vertical */
  }
  
  .task-actions {
      display: flex; /* Flex pour aligner les boutons */
      justify-content: flex-end; /* Alignement à droite */
      gap: 10px; /* Espacement entre les boutons */
  }
  
  .btn {
      background-color: #007bff; /* Couleur de fond du bouton */
      color: #fff; /* Couleur du texte du bouton */
      border: none; /* Pas de bordure */
      border-radius: 0.25rem; /* Arrondi des coins */
      padding: 5px 10px; /* Espacement interne */
      cursor: pointer; /* Curseur indicatif */
      transition: background-color 0.2s; /* Transition pour la couleur de fond */
  }
  
  .btn:hover {
      background-color: #0056b3; /* Couleur au survol */
  }
  </style>
<template>
  <router-view />
</template>

<script setup>
// App.vue is kept minimal as a container for router views
</script>

<style>
/* Global styles */
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap');

:root {
  --primary: #1867C0;
  --primary-dark: #135bb3;
  --secondary: #5CBBF6;
  --success: #4CAF50;
  --error: #FF5252;
  --warning: #FFC107;
  --info: #2196F3;
  --dark: #1E1E1E;
  --light: #F5F5F5;
  --gray: #9E9E9E;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Inter', sans-serif;
  background-color: var(--light);
  color: var(--dark);
}

.dark-mode {
  background-color: var(--dark);
  color: var(--light);
}
</style>
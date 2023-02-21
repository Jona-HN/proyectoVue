<template>
  <v-table
    fixed-header>
    <thead>
      <tr>
        <th class="text-left">
          Nombre
        </th>
        <th class="text-left">
          Título
        </th>
        <th class="text-left">
          Post
        </th>
      </tr>
    </thead>
    <tbody>
      <tr
        v-for="post in posts"
        :key="post"
      >
        <td>{{ post.nombre }}</td>
        <td>{{ post.title }}</td>
        <td>{{ post.body }}</td>
      </tr>
    </tbody>
  </v-table>
</template>

<script setup>
  import { ref, onMounted } from 'vue';

  const posts = ref([]);
  onMounted(() => {
    fetch('https://jsonplaceholder.typicode.com/posts/')
      .then(response => response.json())
      .then(json => {
        posts.value = json;
        posts.value.forEach(post => {
          fetch(`https://jsonplaceholder.typicode.com/users/${post.userId}`)
            .then(response => response.json())
            .then(json => {
              post.nombre = json.name;
            });
        })
      });
  });
</script>
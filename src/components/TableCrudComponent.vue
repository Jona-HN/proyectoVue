<template>
  <v-data-table
    :headers="headers"
    :items="posts"
    :sort-by="[{ key: 'id', order: 'asc' }]"
    :group-by="groupingKey"
    class="elevation-1"
    fixed-header>

    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>Mi blog</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ props }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="props"
            >
              Nuevo post
            </v-btn>
          </template>
          
          <v-card>
            <v-card-title>
              <span class="text-h5">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                  <v-text-field
                      v-model="editedPost.name"
                      label="Nombre"
                    ></v-text-field>
                </v-row>
                <v-row>
                  <v-text-field
                      v-model="editedPost.title"
                      label="Título"
                    ></v-text-field>
                </v-row>
                <v-row>
                  <v-text-field
                      v-model="editedPost.body"
                      label="Post"
                    ></v-text-field>
                </v-row>
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="close"
              >
                Cancel
              </v-btn>
              <v-btn
                color="blue-darken-1"
                variant="text"
                @click="save"
              >
                Save
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>

        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="text-h5">¿Está seguro de eliminar este ítem?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue-darken-1" variant="text" @click="closeDelete">Cancelar</v-btn>
              <v-btn color="blue-darken-1" variant="text" @click="deletePostConfirm">Confirmar</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    
    <template v-slot:item.actions="{ item }">
      <v-icon
        size="small"
        class="me-2"
        @click="editPost(item.raw)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        size="small"
        @click="deletePost(item.raw)"
      >
        mdi-delete
      </v-icon>
    </template>

    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Reset
      </v-btn>
    </template>
  </v-data-table>
</template>

<script setup>
  import { ref, onMounted, computed, watch, nextTick } from 'vue';

  const posts = ref([]);
  const id = ref(0);
  const groupingKey = ref([{ key: 'name' }]);
  const headers = ref([
    {
      title: 'Id',
      key: 'id',
    },
    {
      title: 'Nombre',
      key: 'name'
    },
    {
      title: 'Título',
      key: 'title',
      sortable: false
    },
    {
      title: 'Post',
      key: 'body',
      sortable: false
    },
    {
      title: 'Acciones',
      key: 'actions',
      sortable: false
    },
  ]);
  
  const dialog = ref(false);
  const dialogDelete = ref(false);

  watch(dialog, (val) => {
    val || close();
  });

  watch(dialogDelete, (val) => {
    val || closeDelete();
  });

  const editedIndex = ref(-1);
  const editedPost = ref({
    id: 0,
    nombre: '',
    title: '',
    body: ''
  });
  const defaultPost = {
    id: 0,
    nombre: '',
    title: '',
    body: ''
  };

  const formTitle = computed({
    get() {
      return editedIndex.value === -1 ? 'Nuevo post' : 'Editar post';
    }
  })

  const close = async () => {
    dialog.value = false;
    await nextTick(() => {
      editedPost.value = Object.assign({}, defaultPost.value);
      editedIndex.value = -1;
    })
  }

  const save = () => {
    if (editedIndex.value > -1) {
    
      // Formas de actualizar los valores del post editado:

      // 1. Asignado el objeto con los nuevos valores
      //    al objeto ya existente
      Object.assign(posts.value[editedIndex.value], editedPost.value);
      
      // 2. Mediante el método PUT que ofrece la API
      fetch('https://jsonplaceholder.typicode.com/posts/1', {
        method: 'PUT',
        headers: {'content-type': 'application/json'},
        body: JSON.stringify(editedPost.value)
      })
        .then(response => response.json())
        .then(json => console.log(json))
    }
    else {
      let lastPost = posts.value.slice(-1).at(0);
      editedPost.value.id = lastPost.id + 1;

      // Formas de agregar el nuevo post:
      // 1. Agregándolo al arreglo de posts
      posts.value.push(editedPost.value);

      // 2. Mediante el método POST que ofrece la API
      fetch('https://jsonplaceholder.typicode.com/posts/', {
        method: 'POST',
        headers: {'content-type': 'application/json'},
        body: JSON.stringify(editedPost.value)
      })
        .then(response => response.json())
        .then(json => console.log(json))
    }

    close();
  }

  const closeDelete = async () => {
    dialogDelete.value = false;
    await nextTick(() => {
      editedPost.value = Object.assign({}, defaultPost.value);
      editedIndex.value = -1;
    })
  }

  const deletePostConfirm = () => {
    posts.value.splice(editedIndex.value, 1);

    closeDelete();
  }

  const editPost = (post) => {
    editedIndex.value = posts.value.indexOf(post);
    editedPost.value = Object.assign({}, post);
    dialog.value = true;
  }

  const deletePost = (post) => {
    editedIndex.value = posts.value.indexOf(post);
    editedPost.value = Object.assign({}, post);
    dialogDelete.value = true;
  }

  onMounted(initialize);

  function initialize() {
    fetch('https://jsonplaceholder.typicode.com/posts/')
      .then(response => response.json())
      .then(json => {
        posts.value = json;
        posts.value.id = ++id.value;
        posts.value.forEach(post => {
          fetch(`https://jsonplaceholder.typicode.com/users/${post.userId}`)
            .then(response => response.json())
            .then(json => {
              post.name = json.name;
            });
        })
      });
  }
</script>
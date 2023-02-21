<template>
  <v-container>
    <v-row justify="space-around">
      <v-card width="400">
        <v-img
          height="200"
          lazy-src="https://img.freepik.com/premium-vector/system-software-update-upgrade-concept-loading-process-screen-vector-illustration_175838-2182.jpg?w=2000"
          :src=background
          cover
          class="text-white"
        >
          <v-toolbar
            color="rgba(0, 0, 0, 0)"
            theme="dark"
          >
            <template v-slot:prepend>
              <v-btn icon="$menu"
              ></v-btn>
            </template>

            <v-toolbar-title class="text-h6">
              Messages
            </v-toolbar-title>

            <template v-slot:append>
              <v-btn icon="mdi-dots-vertical"></v-btn>
            </template>
          </v-toolbar>
          <template v-slot:placeholder>
            <div class="d-flex align-center justify-center fill-height">
              <v-progress-circular
                color="grey-lighten-4"
                indeterminate
              ></v-progress-circular>
            </div>
          </template>
        </v-img>

        <v-card-text>
          <div class="font-weight-bold ms-1 mb-2">
            Today
          </div>

          <v-timeline density="compact" align="start">
            <v-timeline-item
              v-for="message in messages"
              :key="message.time"
              :dot-color="message.color"
              size="x-small"
            >
              <div class="mb-4">
                <div class="font-weight-normal">
                  <strong>{{ message.from }}</strong> @{{ message.time }}
                </div>
                <div>{{ message.message }}</div>
              </div>
            </v-timeline-item>
          </v-timeline>
        </v-card-text>
        <v-card-actions>
          <v-btn @click="recargaImagen" :color="red">
            Reload
          </v-btn>
        </v-card-actions>

        <v-expand-transition>
      <div v-show="show">
        <v-divider></v-divider>

        <v-card-text>
          I'm a thing. But, like most politicians, he promised more than he could deliver. You won't have time for sleeping, soldier, not with all the bed making you'll be doing. Then we'll go with that data file! Hey, you add a one and two zeros to that or we walk! You're going to do his laundry? I've got to find a way to escape.
        </v-card-text>
      </div>
    </v-expand-transition>
      </v-card>
    </v-row>
  </v-container>
</template>

<script setup>
  import { ref } from 'vue';

  const red = ref("blue")
  const messages = ref([]);

  const background = ref("");

  messages.value = [
    {
      from: 'You',
      message: `Sure, I'll see you later.`,
      time: '10:42am',
      color: 'deep-purple-lighten-1',
    },
    {
      from: 'John Doe',
      message: 'Yeah, sure. Does 1:00pm work?',
      time: '10:37am',
      color: 'green',
    },
    {
      from: 'You',
      message: 'Did you still want to grab lunch today?',
      time: '9:47am',
      color: 'deep-purple-lighten-1',
    },
  ]

  recargaImagen()

  async function recargaImagen() {
    if (red.value == "blue") {
      red.value = "red"
    }
    else {
      red.value = "blue"
    }

    let promise = await fetch("https://random.imagecdn.app/400/200");
    background.value = promise.url
  }
</script>

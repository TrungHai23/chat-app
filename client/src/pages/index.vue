<template>
  <VContainer class="index">
    <VForm @submit.prevent="onSubmit">
      <VCard max-width="600" class="mx-auto pa-5">
        <VCardTitle>
          <div class="d-flex align-center justify-center">
            <VIcon icon="mdi-chat" />
            <h3 class="ml-2">Vue Chatapp</h3>
          </div>
        </VCardTitle>
        <VCardText class="py-4">
          <VTextField label="Username" v-model="state.username"></VTextField>
          <VSelect :items="rooms" label="Room" v-model="state.room"></VSelect>
        </VCardText>
        <VCardActions>
          <VBtn
            block
            color="primary"
            :disabled="!state.username || !state.room"
            variant="outlined"
            type="submit"
          >
            Join Chatapp
          </VBtn>
        </VCardActions>
      </VCard>
    </VForm>
  </VContainer>
</template>

<script lang="ts" setup>
import { ref, onMounted, onBeforeUnmount, reactive } from "vue";
import { io, type Socket } from "socket.io-client";
import { useRouter } from 'vue-router';

const socket = ref<Socket>();

onMounted(() => {
  socket.value = io("http://localhost:3001");
  socket.value?.on("message", (response: any) => {
    console.log(response);
  });
});

onBeforeUnmount(() => {
  console.log("DISCONNECT_BLOCK");
  socket.value?.disconnect();
});

const router = useRouter();
const rooms = ['vue installation', 'vue guide', 'vue api', 'vue examples'];
const state = reactive({
  username: '',
  room: rooms[0],
});
const onSubmit = () => {
  console.log('[SUBMIT]');
  router.push(`/chat?username=${state.username}&room=${state.room}`);
};
</script>

<style scoped>
</style>

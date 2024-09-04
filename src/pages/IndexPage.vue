<template>
  <q-page>
    <q-list v-if="tasks.length > 0" bordered>
      <q-item v-for="task in tasks" :key="task.id">
        <q-item-section>
          <q-checkbox v-model="task.completed" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section side>
          <q-btn icon="edit" @click="edit(task.id)" />
          <q-btn icon="delete" @click="del(task.id)" />
        </q-item-section>
      </q-item>
    </q-list>
    <q-page-sticky position="bottom-right">
      <q-btn fab color="primary" icon="add" @click="prompt" />
    </q-page-sticky>
  </q-page>
</template>

<script setup lang="ts">
import { useStorage } from '@vueuse/core';
import { useQuasar } from 'quasar';
import { onMounted } from 'vue';
import { Todo } from './model';

defineOptions({
  name: 'IndexPage',
});

const $q = useQuasar();

let tasks = useStorage('tasksx', [] as Todo[]);

function prompt() {
  $q.dialog({
    title: 'Prompt',
    message: 'Título de la tarea:',
    prompt: {
      model: '',
      type: 'text',
    },
    cancel: true,
    persistent: true,
  }).onOk((data) => {
    tasks.value.push({
      id: tasks.value.length + 1,
      title: data,
      completed: false,
    });
  });
}

function del(id: number) {
  tasks.value = tasks.value.filter((task) => task.id !== id);
}

function edit(id: number) {
  const task = tasks.value.find((task) => task.id === id);
  $q.dialog({
    title: 'Edit',
    message: 'Título de la tarea:',
    prompt: {
      model: task!.title,
      type: 'text',
    },
    cancel: true,
    persistent: true,
  }).onOk((data) => {
    task!.title = data;
  });
}

onMounted(() => {
  $q.notify({
    message: 'Hello, World!',
    color: 'primary',
  });
});
</script>

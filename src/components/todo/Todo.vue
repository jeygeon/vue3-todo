<template>
  <v-app>
    <v-container style="max-width: 550px">
      <!-- Input Todo -->
      <v-text-field
        label="Add Todo"
        variant="underlined"
        v-model="newTodo"
        @keydown.enter="addTodo"
      ></v-text-field>

      <!-- Total Todo -->
      <h2 class="text-h4 text-success ps-4">
        Tasks:&nbsp;
        <v-slide-y-transition leave-absolute>
          <!--
          꼭 key가 들어가 있어야 한다.
          1. key가 없으면 변화가 제대로 감지되지 않아 트랜지션이 동작하지 않을 수 있다.
          2. Vue에서는 :key 항목을 고유하게 식별하기 때문에 해당 값이 없으면 요소를 새로 그리기 때문에 성능저하가 생길 수 있다.
            > :key값이 존재하지 않을 경우 요소의 값이 변경되어도 DOM 자체가 새로 렌더링 되지 않기 때문에 (내부 데이터만 변경)
              원하는 애니메이션이 동작하지 않을 것 이다.
          -->
          <span :key="`todo-${todos.length}`"> {{ todos.length }} </span>
        </v-slide-y-transition>
      </h2>

      <!-- Todo status -->
      <v-divider class="mt-4"></v-divider>
      <v-row no-gutters align="center" class="mt-2 mb-1">
        <v-col cols="3">
          <strong>남은 todo : {{ remainedTodos }}</strong>
        </v-col>
        <v-divider class="mx-3" vertical></v-divider>
        <v-col>
          <strong>완료 todo : {{ complatedTodos }}</strong>
        </v-col>
      </v-row>

      <!-- Todo progress bar -->
      <v-progress-linear
        v-model="progress"
        color="purple-accent-4"
        :width="2"
        :height="5"
        class="mb-3"
      ></v-progress-linear>

      <!-- Todo list -->
      <v-card v-if="todos.length > 0">
        <template v-for="(todo, i) in todos" :key="i">
          <v-list-item @click="todo.done = !todo.done">
            <template v-slot:prepend>
              <v-checkbox-btn v-model="todo.done" color="grey"></v-checkbox-btn>
            </template>
            <v-list-item-title>
              <span :class="todo.done ? 'text-grey' : 'text-primary'">
                {{ todo.text }}
              </span>
            </v-list-item-title>
            <template v-slot:append> </template>
          </v-list-item>
        </template>
      </v-card>
    </v-container>
  </v-app>
</template>

<script setup>
import { computed, ref } from 'vue';
const todos = ref([
  {
    done: false,
    text: 'Foobar',
  },
  {
    done: false,
    text: 'Fizzbuzz',
  },
]);

const newTodo = ref(null);
const complatedTodos = computed(() => {
  return todos.value.filter(todo => todo.done).length;
});
const remainedTodos = computed(() => {
  return todos.value.filter(todo => !todo.done).length;
});
const progress = computed(() => {
  return (complatedTodos.value / todos.value.length) * 100;
});

const addTodo = () => {
  if (newTodo.value == null) {
    alert('todo를 입력해주세요');
    return false;
  }
  todos.value.push({
    done: false,
    text: newTodo.value,
  });
  newTodo.value = null;
};
</script>

<style lang="scss" scoped></style>

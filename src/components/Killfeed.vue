<template>
  <div class="killfeed">
    <transition-group
      name="killfeed-item"
      tag="div"
      class="killfeed-item-wrapper"
      appear
      appear-active-class="killfeed-item-enter-active"
    >
      <div
        v-for="(item, index) in killfeedItems.slice().reverse()"
        :key="item.id"
        :class="['killfeed-item mb-2', item.type]"
      >
        <span class="user"> {{ item.killer }}</span>
        <span class="user"> ‎ </span>
        <kbd
          class="font text-md font-semibold text-gray-800 bg-gray-100 border border-gray-200 rounded-lg dark:bg-gray-600 dark:text-gray-100 dark:border-gray-500"
          ><span class="user"> ‎ </span>{{ item.weapon }}
          <i class="nf-fa-crosshairs"> </i><span class="user"> ‎ </span
          ><span class="user"> ‎ </span>
        </kbd>
        <span class="user"> ‎ </span>
        <span class="user"> ‎ </span>
        <span class="user"> {{ item.victim }}</span>
      </div>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: "Killfeed",
  props: {
    killfeedItems: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style scoped>
.font {
  font-family: "HurmitNerdFont", sans-serif;
  font-style: normal;
  font-weight: 100;
}

.killfeed {
  position: fixed;
  top: 50%;
  right: 20px;
  transform: translateY(-50%);
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  z-index: 0;
  max-width: 300px;
  padding: 10px 0;
}

.killfeed-item-wrapper {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  margin-bottom: 5px;
}

.killfeed-item,
.killfeed-item-me-die,
.killfeed-item-me {
  background: linear-gradient(to right, #353535, #97959531);
  color: #ffffff;
  padding: 5px 15px;
  border-radius: 6px;
  border: #531516;
  width: max-content;
  max-width: 100%;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
  display: flex;
  justify-content: flex-end;
  align-items: center;
  z-index: 0;
  opacity: 0;
  transform: translateX(100%);
  animation: killfeed-enter 0.3s ease-out forwards;
}

.killfeed-item-me {
  background: linear-gradient(to right, #881e20, #1818183a);
}

.killfeed-item-me-die {
  background: linear-gradient(to right, #1818183a, #881e20);
}

.icon {
  margin: 0 5px;
}

.user {
  margin-right: 4px;
  margin-left: 6px;
  z-index: 0;
}

@keyframes killfeed-enter {
  0% {
    opacity: 0;
    transform: translateX(100%);
  }
  100% {
    opacity: 1;
    transform: translateX(0);
  }
}

@keyframes killfeed-exit {
  0% {
    opacity: 1;
    transform: translateX(0);
  }
  100% {
    opacity: 0;
    transform: translateX(100%);
  }
}

.killfeed-item-leave-active {
  animation: killfeed-exit 0.3s ease-out forwards;
}

.killfeed-item-enter-active {
  animation: killfeed-enter 0.3s ease-out forwards;
}

.killfeed-item-enter-from {
  opacity: 0;
  transform: translateX(100%);
}
</style>

<template>
  <div class="track-info-main">
    <h2 class="name-title">{{ name || "Скоро здесь будет название" }}</h2>
    <!-- нужно изменить isNotAssigned на isAssigned-->
    <!-- когда будет реализовано назначение курсов -->
    <div v-if="isNotAssigned" class="track-ordered-progress">{{completionPercent}}% изучено</div>

    <div class="description">
      {{ description || fishText }}
    </div>
  </div>
</template>

<script>
export default {
  name: 'TrackInfoMain',
  props: {
    name: {
      type: String,
    },
    description: {
      type: String,
    },
    isNotAssigned: {
      type: Boolean,
    },
    completionPercent: {
      type: Number,
    },
  },
  data() {
    return {
      fishText:
        'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
    };
  },
};
</script>

<style scoped lang="scss">
.track-info-main {
  text-align: left;
  display: flex;
  flex-direction: column;
  gap: 24px;

}

//ordered
.track-ordered-progress {
  font-weight: bold;
  color: #6FCF97;
  font-size: 18px;
}
</style>

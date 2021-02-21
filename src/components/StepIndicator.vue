<template>
  <div class="step-indicator">
    <div class="progress-bar">
      <div class="progress" :style="{width: `${currentStep * (100 / (total - 1))}%`}"></div>
    </div>
    <div v-for="step in 3" :key="step" class="step" :class="{'active' : currentStep === step - 1, 'valid' : validSteps[step - 1]}">
      <span>{{ step }}</span>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      currentStep: Number,
      total: Number,
      validSteps: Object
    }
  }
</script>

<style lang="scss" scoped>
  .step-indicator {
    display: flex;
    justify-content: space-between;
    position: relative;
    margin-bottom: 2rem;
  }

  .step {
    width: 3rem;
    height: 3rem;
    display: flex;
    align-items: center;
    justify-content: center;
    background: var(--white);
    border: 2px solid var(--white);
    color: var(--darkgrey);
    border-radius: 50%;
    z-index: 2;
    position: relative;
    font-family: ubuntu;
  }

  .active {
    border: 2px solid var(--pink);
    color: var(--pink);
  }

  .valid {
    border: 2px solid var(--pink);
    background-color: var(--pink);
    color: var(--white);
  }

  .progress-bar {
    width: calc(100% - 4px);
    height: 8px;
    border-radius: 4px;
    margin: 0 2px;
    background: rgba(255,255,255,0.3);
    position: absolute;
    top: calc(50% - 5px);
  }

  .progress {
    width: 0;
    height: 100%;
    margin-top: 2px;
    background: var(--white);
    transition: ease width .4s;
  }
</style>
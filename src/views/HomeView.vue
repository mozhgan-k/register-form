<script setup lang="ts">
import { ref } from "vue";

const activeStep = ref(1);
const steps = [
  { step: 1, title: "Username" },
  { step: 2, title: "Email" },
  { step: 3, title: "Review" },
]

const username = ref("")
const email = ref("")

// Methods
const nextStep = () => {
  if (activeStep.value < 3) {
    activeStep.value += 1;
  }
};

const prevStep = () => {
  if (activeStep.value > 1) {
    activeStep.value -= 1;
  }
};

const setActiveStep = (step: number) => {
  activeStep.value = step;
};
</script>

<template>
  <main style="max-width: 900px; margin-inline: auto;">
    <div class="wrapper">
      <ol class="c-stepper">
        <template v-for="step in steps" :key="step.step">
          <li class="c-stepper__item" :class="activeStep >= step.step ? 'text-primary' : 'text-grey'"
            @click="setActiveStep(step.step)"
            >
            <div class="c-stepper__icon" :class="activeStep >= step.step ? 'active' : ''">
              <!-- <component :is="step.icon" :width="24" :height="24" /> -->
            </div>
            <span class="c-stepper__title">
              {{ step.title }}
            </span>
          </li>
        </template>
      </ol>
    </div>
    <form @submit.prevent="nextStep">
      <!-- Step 1: Username -->
      <section v-if="activeStep === 1">
        <label for="username">
          Username
          <input class="form-input" id="username" v-model="username" />
        </label>
      </section>

      <!-- Step 2: Email -->
      <section v-if="activeStep === 2">
        <label for="email">
          Email
          <input class="form-input" id="email" v-model="email" type="email" />
        </label>
      </section>

      <!-- Step 3: Review Section -->
      <section v-if="activeStep === 3">
        <div>Entered Username is: {{ username }}</div>
        <div>Entered Email is: {{ email }}</div>
      </section>

      <!-- Navigation Buttons -->
      <div class="form-navigation">
        <button class="btn" id="btn-prev" :disabled="activeStep === 1" @click="prevStep" type="button">
          Previous
        </button>
        <button class="btn bg-primary" id="btn-next" :disabled="activeStep === 3" type="submit">
          Next
        </button>
      </div>
    </form>
  </main>
</template>

<style scoped>

/* STEPPER STYLES */
:root {
  --circle-size: clamp(1.5rem, 5vw, 3rem);
  --spacing: clamp(0.25rem, 2vw, 0.5rem);
}

ol {
  padding: 0;
}

.active {
  background-color: rgba(98, 89, 202, 0.1);
}

.c-stepper {
  display: flex;
}

.c-stepper__item {
  display: flex;
  gap: var(--spacing);
  align-items: center;
  cursor: pointer;
}

.c-stepper__item::before {
  content: "";
  display: block;
  width: var(--circle-size);
  height: var(--circle-size);
  border-radius: 50%;
  background-color: lightgrey;
}

.c-stepper__item:not(:last-child) {
  flex: 1;
}

.c-stepper__item:not(:last-child)::after {
  content: "";
  position: relative;
  height: 2px;
  background-color: currentColor;
  flex: 1;
  margin-right: 0.5rem;
  margin-left: 0.5rem;
}

.c-stepper__icon {
  height: 38px;
  width: 38px;
  border-radius: 50%;
  border: 1px solid currentColor;
  display: flex;
  align-items: center;
  justify-content: center;
}

.c-stepper__title {
  text-align: right;
  font-size: 16px;
  font-weight: 700;
  line-height: 25px;
  margin-left: 10px;
}

.wrapper {
  width: 100%;
  background-color: #fff;
  border-radius: 10px;
  padding: 20px;
}

/* FORM STYLES */

form {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  margin-top: 20px;
}

.form-navigation {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  margin-top: 20px;
}


#btn-prev {
  border: 1px solid #ccc;
}

#btn-next {
  color: #fff;
  margin-left: 10px;
}

</style>
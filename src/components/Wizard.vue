<script setup lang="ts">
import {ref} from 'vue';
import IconUser from "@/components/icons/IconUser.vue";
import IconEmail from "@/components/icons/IconEmail.vue";
import IconUserId from "@/components/icons/IconUserId.vue";

const steps = [
  {step: 1, title: "Username", icon: IconUser},
  {step: 2, title: "Email", icon: IconEmail},
  {step: 3, title: "Review", icon: IconUserId},
]

// Reactive state
const activeStep = ref(1);
const username = ref('');
const email = ref('');
const usernameError = ref('');
const emailError = ref('');

// Validation functions
const validateUsername = () => {
  usernameError.value = '';
  if (!username.value) {
    usernameError.value = 'Invalid Username.';
    return;
  }
  if (username.value.includes('@') || username.value.includes(' ')) {
    usernameError.value = 'Invalid Username.';
    return;
  }
  if (username.value.length < 4 || username.value.length > 20) {
    usernameError.value = 'Invalid Username.';
    return;
  }
  activeStep.value = 2; // Move to the next step
};

const validateEmail = () => {
  emailError.value = '';
  const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  if (!email.value) {
    emailError.value = 'Invalid email address.';
    return;
  }
  if (!emailPattern.test(email.value)) {
    emailError.value = 'Invalid email address.';
    return;
  }
  activeStep.value = 3; // Move to the next step
};

// Navigation functions
const prevStep = () => {
  if (activeStep.value > 1) {
    activeStep.value--;
  }
};

</script>

<template>
  <div>
    <div class="wrapper">
      <ol class="c-stepper">
        <template v-for="step in steps" :key="step.step">
        <li class="c-stepper__item" :class="activeStep >= step.step  ? 'text-primary' : 'text-grey'"
        >
          <div class="c-stepper__icon" :class="activeStep >= step.step ? 'active' : ''">
            <Component :is="step.icon" width="24px" height="24px"/>
          </div>
          <span class="c-stepper__title">
            {{step.title}}
          </span>
        </li>
        </template>
      </ol>
    </div>
    <!-- Step 1: Username -->
    <div class="form" v-if="activeStep === 1">
      <h2>Username:</h2>
      <input
          id="username"
          v-model="username"
          type="text"
          class="form-input"
          placeholder="Enter your username"
      />
      <p v-if="usernameError" class="error">{{ usernameError }}</p>
      <div class="form-navigation">
        <button class="btn outlined-btn" id="btn-prev" :disabled="activeStep === 1" @click="prevStep">Prev</button>
        <button class="btn bg-primary" id="btn-next" @click="validateUsername">Next</button>
      </div>
    </div>

    <!-- Step 2: Email -->
    <div class="form" v-if="activeStep === 2">
      <h2>Email:</h2>
      <input
          id="email"
          class="form-input"
          v-model="email"
          type="text"
          placeholder="Enter your email"
      />
      <p v-if="emailError" class="error">{{ emailError }}</p>
      <div class="form-navigation">
        <button class="btn outlined-btn" id="btn-prev" @click="prevStep">Prev</button>
        <button class="btn bg-primary" id="btn-next" @click="validateEmail">Next</button>
      </div>
    </div>

    <!-- Step 3: Review -->
    <div class="form" v-if="activeStep === 3">
      <h2>Step: review</h2>
      <p>Username: {{ username }}</p>
      <p>Email: {{ email }}</p>
      <div class="form-navigation">
        <button class="btn outlined-btn" id="btn-prev" @click="prevStep">Prev</button>
        <button class="btn bg-primary" id="btn-next" disabled>Next</button>
      </div>
    </div>
  </div>
</template>

<style>

#btn-next {
  color: #fff;
  margin-left: 10px;
}

.form-navigation {
  display: flex;
  justify-content: flex-end;
  align-items: center;
  margin-top: 20px;
}

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

.form {
  background-color: #fff;
  padding: 20px;
  border-radius: 10px;
  margin-top: 20px;
}
</style>
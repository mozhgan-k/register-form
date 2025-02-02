<script setup lang="ts">
import {ref} from "vue";

const inputText = ref('')
const isBalancedResult = ref(null)
const balancedMessage = ref('The text is balanced.')
const unbalancedMessage = ref('The text is not balanced.')

const checkBalance = () => {
  isBalancedResult.value = isBalanced(inputText.value);
}
const isBalanced = (text: string) => {
  const stack = [];
  const pairs = {
    '(': ')',
    '[': ']',
    '{': '}',
  };

  for (let char of text) {
    if (pairs[char]) {
      stack.push(char);
    } else if (char === ')' || char === ']' || char === '}') {
      if (pairs[stack.pop()] !== char) {
        return false;
      }
    }
  }

  return stack.length === 0;
}
</script>

<template>
  <div>
    <input
        id="description"
        v-model="inputText"
        class="form-input"
        @input="checkBalance"
        placeholder="Enter text"
    />
    <p v-if="isBalancedResult !== null">
      {{ isBalancedResult ? balancedMessage : unbalancedMessage }}
    </p>
  </div>
</template>

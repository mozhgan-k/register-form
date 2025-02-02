<template>
  <div>
    <input
        id="description"
        type="text"
        v-model="inputText"
        @input="debouncedCheckBalance"
        class="form-input"
        placeholder="Enter text"
    />
    <p class="error">{{ message }}</p>
  </div>
</template>

<script>
/* USE OPTION API BASED ON DESCRIPTION TEST FILE */
export default {
  data() {
    return {
      inputText: '',
      message: '',
      debounceTimeout: null, // add debounce to fix multiple call checkBalance()
    };
  },
  methods: {
    debouncedCheckBalance() {
      // Clear the previous timeout
      if (this.debounceTimeout) {
        clearTimeout(this.debounceTimeout);
      }
      // Set a new timeout to call checkBalance after 300ms
      this.debounceTimeout = setTimeout(() => {
        this.checkBalance();
      }, 300);
    },
    checkBalance() {
      this.message = this.isBalanced(this.inputText)
          ? 'The text is balanced.'
          : 'The text is not balanced.';
    },
    isBalanced(text) {
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
    },
  },
};
</script>

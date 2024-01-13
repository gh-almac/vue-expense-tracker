<template>
    <!-- Displaying the income and expenses -->
    <div class="inc-exp-container">
        <div>
          <h4>Income</h4>
          <!-- Displaying the income value passed as a prop -->
          <p id="money-plus" class="money plus">+${{ income }}</p>
        </div>
        <div>
          <h4>Expense</h4>
          <!-- Displaying the expenses value passed as a prop -->
          <p id="money-minus" class="money minus">-${{ expenses }}</p>
        </div>
    </div>
</template>

<script setup>
    import { defineProps } from 'vue';

    //defining the props that will be passed to this component
    const props = defineProps({
        //the income and expenses props are required and must be numbers
        income: {
            type: Number,
            required: true,
        },
        expenses: {
            type: Number,
            required: true,
        },
    });
</script>
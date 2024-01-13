<template>
    <!-- Displaying the transaction history -->
    <h3>History</h3>
    <!-- Instructions for editing a transaction -->
    <p>To edit a transaction click on the text or amount.</p>
    <!-- List of transactions -->
    <ul id="list" class="list">
        <!-- Looping through each transaction -->
        <li v-for="transaction in transactions"
            :key="transaction.id"
            :class="transaction.amount < 0 ? 'minus' : 'plus'">

            <!-- Displaying the transaction text -->
            <!-- If the transaction is not being edited, or if the input to toggle is not the description of this transaction or is null, display the text as a span -->
            <span v-if="editId !== transaction.id && (inputToToggle !== 'description'+transaction.id || inputToToggle === null)"
            @click="editTransaction(transaction.id, 'description'+transaction.id)">
                {{ transaction.text }}
            </span>
            <!-- If the transaction is being edited, display the text as an input field -->
            <input v-else type="text" v-model="transaction.text"
                class="input-field-long" @blur="submitChange" />

            <!-- Displaying the transaction amount -->
            <!-- If the transaction is not being edited, or if the input to toggle is not the amount of this transaction or is null, display the amount as a span -->
            <span v-if="editId !== transaction.id && (inputToToggle !== 'amount'+transaction.id || inputToToggle === null)"          @click="editTransaction(transaction.id, 'amount'+transaction.id)">
                ${{ transaction.amount }}
            </span>
            <!-- If the transaction is being edited, display the amount as an input field -->
            <input v-else type="number" v-model="transaction.amount" class="input-field-short" @blur="submitChange" />

            <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
    </ul>
</template>

<script setup>
    import { ref, defineProps } from 'vue';

    const emit = defineEmits(['transactionDeleted','transactionUpdated']);

    // Define the props that this component accepts
    const props = defineProps({
        transactions: Array,
        required: true,
    });

    // Function to delete a transaction
    // Emits a 'transactionDeleted' event with the id of the transaction to be deleted
    const deleteTransaction = (id) => {
        emit('transactionDeleted', id);
    };

    // Function to edit a transaction
    // Sets the 'editId' ref to the id of the transaction to be edited
    // Sets the 'inputToToggle' ref to the input field to be toggled
    const editTransaction = (id, input) => {
        editId.value = id;
        inputToToggle.value = input;
        console.log(inputToToggle.value);
    };

    // Function to submit the changes made to a transaction
    // Finds the transaction with the matching id in the 'transactions' array
    // If the transaction is found, emits a 'transactionUpdated' event with the id of the transaction and the new amount
    // Sets the 'editId' ref to null to hide the input field
    const submitChange = () => {
        const transaction = props.transactions.find(transaction => transaction.id === editId.value);

        if(transaction) {
            emit('transactionUpdated', editId.value, transaction.amount);
        }
        editId.value = null;
    };

    // Ref to store the id of the transaction being edited
    // Initially set to null
    const editId = ref(null);
    const inputToToggle = ref(null);
</script>

<style scoped>
.list {
  list-style-type: none;
  padding: 0;
  margin-bottom: 40px;
}

.list li {
  background-color: #fff;
  box-shadow: var(--box-shadow);
  color: #333;
  display: flex;
  justify-content: space-between;
  position: relative;
  padding: 10px;
  margin: 10px 0;
}

.list li.plus {
  border-right: 5px solid #2ecc71;
}

.list li.minus {
  border-right: 5px solid #c0392b;
}

.delete-btn {
  cursor: pointer;
  background-color: #e74c3c;
  border: 0;
  color: #fff;
  font-size: 20px;
  line-height: 20px;
  padding: 2px 5px;
  position: absolute;
  top: 50%;
  left: 0;
  transform: translate(-100%, -50%);
  opacity: 0;
  transition: opacity 0.3s ease;
}

.list li:hover .delete-btn  {
  opacity: 1;
}

.input-field-short {
    width: 80px; /* Adjust as needed */
    padding:2px;
}

.input-field-long {
    width: 250px; /* Adjust as needed */
    padding:2px;
}
</style>
<template>
    <Header />
    <div class="container">
        <Balance 
            :total="+total"
        />
        <IncomeExpense 
            :income="+income"
            :expense="+expense"
        />
        <TransactionList 
            :transactions="transactions"
            @deleteTrasaction="handledeleteTrasaction"
        />
        <AddTransaction
            @transactionSubmitEvent="handleTransactionSubmitEvent"
        />
    </div>
</template>


<script setup>

import { ref, computed, onMounted } from 'vue';
import Header from '@/components/Header.vue';
import Balance from '@/components/Balance.vue';
import IncomeExpense from '@/components/IncomeExpense.vue';
import TransactionList from '@/components/TransactionList.vue';
import AddTransaction from '@/components/AddTransaction.vue';

import { useToast } from "vue-toastification";

const transactions = ref([
    
]);

onMounted(() => {
    const savedTransactions = JSON.parse(localStorage.getItem('transactions'));

    if (savedTransactions) {
        transactions.value = savedTransactions;
    }
})

const toast = useToast();

//get total

const total = computed (() => {
    return transactions.value.reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0).toFixed(2);
});


//get income

const income = computed (() => {
    return transactions.value.
    filter((transaction) => transaction.amount > 0)
    .reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0).toFixed(2);
});

//get Expense

const expense = computed (() => {
    return transactions.value.
    filter((transaction) => transaction.amount < 0)
    .reduce((acc, transaction) => {
        return acc + transaction.amount;
    }, 0).toFixed(2);
});


//Add transaction

const handleTransactionSubmitEvent = (transactionData) => {
    transactions.value.push({
        id: generateniqueId(),
        text: transactionData.text,
        amount: transactionData.amount
    })
    saveTransactionToStorage();
    toast.success('Transaction Added Successfully.')
}

const generateniqueId = () => {
    return Math.floor(Math.random() * 100000000);
}

const handledeleteTrasaction = (id) => {

    transactions.value = transactions.value.filter ((transaction) => transaction.id != id);
    saveTransactionToStorage();
    toast.success('Transaction Deleted Successfully.');

}

//update local storage

const saveTransactionToStorage = () => {
    localStorage.setItem("transactions", JSON.stringify(transactions.value));
}

</script>

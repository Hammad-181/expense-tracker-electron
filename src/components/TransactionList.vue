<template>
     <ul id="list" class="list">
        <li v-for="transaction in transactions" :key="transaction.id" :class="transaction.amount > 0 ? 'plus' : 'minus'">
            {{transaction.text}} <span>₹{{transaction.amount}}</span>
            <button @click="deleteTransaction(transaction.id)" class="delete-btn">x</button>
        </li>
      </ul>
</template>


<script setup>
import { defineProps } from 'vue';

const emit = defineEmits(["deleteTrasaction"])

const props = defineProps({
    transactions : {
        type: Array,
        required: true
    }
})

const deleteTransaction = (id) => {


emit ("deleteTrasaction", id);

}

</script>

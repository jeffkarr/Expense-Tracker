<template>
    <div>
        <h3>History</h3>
        <ul id="list" class="list">
            <!-- <li class="minus">
                Cash <span>-$400</span><button class="delete-btn">x</button>
            </li>
            <li class="plus">
                Paycheck <span>$800</span><button class="delete-btn">x</button>
            </li> -->
            <li v-for="trxn in transactions" 
                :key="trxn.id"
                :class="trxn.amount < 0 ? 'minus' : 'plus'">
                {{ trxn.text }} <span>{{ trxn.amount}}</span>
                <button class="delete-btn" @click="deleteTransaction(trxn.id)">x</button>
            </li>
        </ul>
    </div>
</template>



<!-- Using the new vue 3 setup script tag way -->
<script setup>
    import {defineProps} from 'vue';

    const emit = defineEmits([
        'transactionDeleted'
    ])
    
    const props = defineProps({
        transactions: {
            type: Array,
            required: true
        }
    });

    const deleteTransaction = (id) => {
        console.log(`deletedTransaction id is ${id}`);
        emit('transactionDeleted', id);
    };

</script>

<!-- <script>
    export default {
        // using the old vue 2 options way:
        // data() {
        //     return {
        //         transactions: [
        //             { id: 1, text: 'Flower', amount: -19.99 },
        //             { id: 2, text: 'Salary', amount: 500.00 },
        //             { id: 3, text: 'Book', amount: -10 },
        //             { id: 4, text: 'Camera', amount: 150 },
        //         ]
        //     }
        // }
        // using the vue 3 setup function way
        // setup() {
        //     const transactions = [
        //         { id: 1, text: 'Flower', amount: -19.99 },
        //         { id: 2, text: 'Salary', amount: 500.00 },
        //         { id: 3, text: 'Book', amount: -10 },
        //         { id: 4, text: 'Camera', amount: 150 },     
        //     ]
        //     return {
        //         transactions
        //     }

        // }
    }
</script> -->


<style scoped>
</style>
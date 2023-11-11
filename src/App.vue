<template>
    <Header />
    <div class="container">
        <Balance :total="total"/>
        <IncomeExpenses :income="+income" :expenses="+expenses" />
        <TransactionList :transactions="transactions" @transactionDeleted="handleTransactionDeleted"/>
        <AddTransaction @transactionsEmitted="handleTransactionSubmitted" />
    </div>
</template>

<script setup>
    import Header from './components/Header.vue';
    import Balance from './components/Balance.vue';
    import IncomeExpenses from './components/IncomeExpenses.vue';
    import TransactionList from './components/TransactionList.vue';
    import AddTransaction from './components/AddTransaction.vue';

    import { useToast} from 'vue-toastification';

    import { ref, computed, onMounted } from 'vue';

    const toast = useToast();

    // const transactions = ref([
    //     { id: 1, text: 'Flower', amount: -19.99 },
    //     { id: 2, text: 'Salary', amount: 500.00 },
    //     { id: 3, text: 'Book', amount: -20 },
    //     { id: 4, text: 'Camera', amount: 150 },
    // ])
    
    const transactions = ref([]);

    onMounted( ()=> {
        const savedTransactions = JSON.parse( localStorage.getItem('transactions') );

        if (savedTransactions) {
            transactions.value = savedTransactions;
        }
    })
    const total = computed(() => {
        return transactions.value
        .reduce( (accumulator, transaction) => {
            return accumulator + transaction.amount;
        }, 0)
    });
    const income = computed(() => {
        return transactions.value
        .filter((transaction) => transaction.amount > 0)
        .reduce((accumulator, transaction) => {
            return accumulator + transaction.amount;
        }, 0)
        .toFixed(2)
    });
    const expenses = computed(() => {
        return transactions.value
        .filter((transaction) => transaction.amount < 0)
        .reduce((accumulator, transaction) => {
            return accumulator + transaction.amount;
        }, 0)
        .toFixed(2)
    });
    const handleTransactionSubmitted = (transactionData) => {
        console.log(transactionData);
        transactions.value.push({
            id: generateUniqueId(),
            text: transactionData.text,
            amount: transactionData.amount
        });

        saveTransactionsToLocalStorage();
        
        toast.success('Transaction added');
    };
    const generateUniqueId = () => { 
        return Math.floor(Math.random() * 1000000);
    };

    const handleTransactionDeleted = (id) => {
        console.log(id);
        transactions.value = transactions.value.filter( 
            (transaction) => transaction.id !== id
        );

        saveTransactionsToLocalStorage();
        
        toast.success('Transaction deleted');
    };

    const saveTransactionsToLocalStorage = () => {
        localStorage.setItem('transactions', JSON.stringify(transactions.value));
    };


</script>

<!-- Old vue 2 way -->
<!-- <script>
    import Header from './components/Header.vue';
    import Balance from './components/Balance.vue';
    import IncomeExpenses from './components/IncomeExpenses.vue';
    import TransactionList from './components/TransactionList.vue';
    import AddTransaction from './components/AddTransaction.vue';



    export default {
        components: {
            Header,
            Balance,
            IncomeExpenses,
            TransactionList,
            AddTransaction
        }  
    }

</script> -->
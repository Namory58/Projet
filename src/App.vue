<script setup lang="ts">
import { ref } from "vue";

const newdate = ref("");
const newdescription = ref("");
const newmontant = ref("");

const TransactionsDB = ref<
  {
    id:Number;
    date: Date;
    description: String;
    montant: BigInt;
  }[]
>([
  { id: 1, description: "Coures", date: new Date(), montant: -150n },
  { id: 2, description: "Salaire", date: new Date(), montant: 650n },
]);

function newTransaction() {
  if (newdate.value != "" && newdescription.value && newmontant.value) {
    const newid = TransactionsDB.value.length + 1;
    TransactionsDB.value.push({
      id: newid,
      date: new Date(newdate.value),
      description: newdescription.value,
      montant: BigInt(Math.floor(Number(newmontant.value) * 100)),
    });
    newdate.value = "";
    newdescription.value = "";
    newmontant.value = "";
  }
}
</script>

<template>
  <table>
    <thead>
      <tr>
        <th>Id</th>
        <th>Description</th>
        <th>Date</th>
        <th>Montant</th>
      </tr>
      <tr v-for="transactions in TransactionsDB">
        <td>{{ transactions.id }}</td>
        <td>{{ transactions.description }}</td>
        <td>
          {{ new Intl.DateTimeFormat("fr-FR").format(transactions.date) }}
        </td>
        <td>
          {{
            new Intl.NumberFormat("fr-FR", {
              style: "currency",
              currency: "Eur",
            }).format(Number(transactions.montant) / 100)
          }}
        </td>
      </tr>
    </thead>
  </table>
  <form @submit.prevent="newTransaction">
    <fieldset>
      <legend>Nouvelle Transactions</legend>

      <label>Date</label><input type="date" v-model="newdate" /><br />
      <label>Description</label
      ><input type="text" v-model="newdescription" /><br />
      <label>Montant</label
      ><input type="number" step="0.01" v-model="newmontant" /><br />
      <input type="submit" value="Ajouter une transaction" />
    </fieldset>
  </form>
</template>

<style scoped>
table {
  border-collapse: collapse;
}
table th,
table td {
  border: 1px solid #ccc;
  padding: 10px;
  text-align: center;
}
/* Style pour le titre h2 */
h2 {
  font-size: 20px;
  margin-bottom: 6px;
  text-align: center;
}
</style>

<template>
<div>
    <h1>Statistik</h1>
    <ul>
        <li v-for="(form, index) in formList" :key="index">{{form.fullName}} -
        {{form.serialNumber}} -
        {{form.productionOrder}} -
        {{form.comment}} -
        {{form.date}} -
        {{form.fullName}} -
        {{form.kastAEB}} -
        {{form.kastBEB}} -
        {{form.kastCEB}} -
        {{form.kastAES}} -
        {{form.kastBES}} -
        {{form.kastCES}} -
        {{form.deviationAEB}} -
        {{form.deviationBEB}} -
        {{form.deviationCEB}} -
        {{form.deviationAES}} -
        {{form.deviationBES}} -
        {{form.deviationCES}}
        </li>
    </ul>

    <div>
      <table>
        <thead>
          <th>Serienummer</th>
          <th>Namn</th>
          <th>Produktionsorder</th>
          <th>Kommentar</th>
        </thead>

      <tbody id="tbody1"></tbody>
      </table>
    </div>


</div>
</template>

<script setup>
import { collection, doc, getDocs, onSnapshot, querySnapshot } from "firebase/firestore";
import {db} from '../boot/firebase'
import {ref} from 'vue'
const formRef = collection(db, 'forms');

const formList = ref([])

onSnapshot(formRef,  (snapshot) => {
  snapshot.forEach((doc) => {
      formList.value.push(doc.data());
  });
});

</script>

<template>
<div>
    <h1>Formulär</h1>
    <q-form ref="form" @submit.prevent="onSubmit" @submit="alert1" greedy>
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="serialNumber"
          bg-color="white"
          class="q-pa-none"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Serienummer"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="fullName"
          bg-color="white"
          class="q-pa-none"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Namn"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="productionOrder"
          bg-color="white"
          class="q-pa-none"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Produktionsorder"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="comment"
          bg-color="white"
          class="q-pa-none"
          outlined
          placeholder="Kommentar"
        />
        <div>
            <q-btn
                type="submit"
                color="green-5"
                label="Registrera"
                no-caps
                class="full-width"
                unelevated
            />
        </div>
    </q-form>
</div>
</template>

<script setup>
import {ref} from 'vue'
import {db} from '../boot/firebase'
import { collection, addDoc } from "firebase/firestore";

const serialNumber = ref("");
const fullName = ref("");
const productionOrder = ref("")
const comment = ref("")
const form = ref(null);

const onSubmit = () => {
  if (form.value != null) {
    form.value.validate().then(async (success) => {
      if (success) {
        try {
            const formData = {
                date: Date.now(),
                serialNumber: serialNumber.value,
                fullName: fullName.value,
                comment: comment.value,
                productionOrder: productionOrder.value
            }

            const docRef = await addDoc(collection(db, "forms"), formData);

            console.log("Document written with ID: ", docRef.id);



        } catch (err) {
          console.log(err)
        }
      }
    });
  }
};

function alert1() {
  alert("Formulär registrerat");
}

</script>

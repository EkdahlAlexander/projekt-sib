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
          style="max-width: 300px"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Serienummer"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="productionOrder"
          bg-color="white"
          class="q-pa-none"
          style="max-width: 300px"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Produktionsorder"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="kastAEB"
          bg-color="white"
          class="q-pa-none"
          style="max-width: 300px"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Kastmått A efter balansering"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="kastBEB"
          bg-color="white"
          class="q-pa-none"
          style="max-width: 300px"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Kastmått B efter balansering"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="kastCEB"
          bg-color="white"
          class="q-pa-none"
          style="max-width: 300px"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Kastmått C efter balansering"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="kastAES"
          bg-color="white"
          class="q-pa-none"
          style="max-width: 300px"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Kastmått A efter svarvning"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="kastBES"
          bg-color="white"
          class="q-pa-none"
          style="max-width: 300px"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Kastmått B efter svarvning"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="kastCES"
          bg-color="white"
          class="q-pa-none"
          style="max-width: 300px"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Kastmått C efter svarvning"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="comment"
          bg-color="white"
          class="q-pa-none"
          style="max-width: 300px"
          outlined
          placeholder="Kommentar"
        />
        <q-input
          no-error-icon
          hide-bottom-space
          v-model="fullName"
          bg-color="white"
          class="q-pa-none"
          style="max-width: 300px"
          outlined
          :rules="[(v) => v.length > 0 || 'Obligatoriskt fält']"
          placeholder="Namn"
        />
        <q-date v-model="date"></q-date>

        <div style="height: 340px;" class="flex flex-center q-my-lg">
          <circular-slider-component @slider-value="updateSliderValue"></circular-slider-component>
        </div>

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
import CircularSliderComponent from 'src/components/CircularSliderComponent.vue'

const serialNumber = ref("");
const fullName = ref("");
const productionOrder = ref("")
const comment = ref("")
const form = ref(null);
const date = ref('2022/03/24')
const kastAEB = ref("")
const kastBEB = ref("")
const kastCEB = ref("")
const kastAES = ref("")
const kastBES = ref("")
const kastCES = ref("")
const deviation = ref(0)

const updateSliderValue = (value) => {
  deviation.value = value
}

const onSubmit = () => {
  if (form.value != null) {
    form.value.validate().then(async (success) => {
      if (success) {
        try {
            const formData = {
                serialNumber: serialNumber.value,
                fullName: fullName.value,
                comment: comment.value,
                productionOrder: productionOrder.value,
                date: date.value,
                kastAEB: kastAEB.value,
                kastBEB: kastBEB.value,
                kastCEB: kastCEB.value,
                kastAES: kastAES.value,
                kastBES: kastBES.value,
                kastCES: kastCES.value,
                deviation: deviation.value

            }

            const docRef = await addDoc(collection(db, "forms"), formData);

            console.log("Document written with ID: ", docRef.id);

            location.reload()



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

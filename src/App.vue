<script setup lang="ts">
import { ref } from 'vue'
import itemContact from './components/itemContact.vue'


const isUpdate = ref(false)
const currentEditId = ref(null as number | null)

const formInput = ref({
  name: '',
  phone: '',
})

const dataContacts = ref([
  {
    id: 1,
    name: 'John Doe',
    phone: '1234567890',
  },
  {
    id: 2,
    name: 'Albert',
    phone: '9876543210',
  },
])

function handleSubmit() {
  if(isUpdate.value && currentEditId.value !== null) {
    //update contact
    const index = dataContacts.value.findIndex((contact) => contact.id === currentEditId.value)
    if(index !== -1) {
      dataContacts.value[index] = {id: currentEditId.value, ...formInput.value}
    }
    isUpdate.value = false
    currentEditId.value = null
  } else {
    //add contact
    const newId = dataContacts.value.length ? Math.max(...dataContacts.value.map((contact) => contact.id)) + 1 : 1
    dataContacts.value.push({
      id: newId,
      ...formInput.value,})
  }

  //reset form
  formInput.value.name = ''
  formInput.value.phone = ''
}

// function addContact() {
//   dataContacts.value.push({
//     id: dataContacts.value.length + 1,
//     name: formInput.value.name,
//     phone: formInput.value.phone,
//   })
// }
function editContact(id: number) {
  const contact = dataContacts.value.find((contact) => contact.id === id)
  if (contact) {
    formInput.value.name = contact.name
    formInput.value.phone = contact.phone
  }
  isUpdate.value = true
  currentEditId.value = id
}
function deleteContact(id: number) {
  dataContacts.value = dataContacts.value.filter((contact) => contact.id !== id)
}
</script>

<template>
  <div>
    <Button>Click Me</Button>
    <h1>Hello World</h1>
    <h2>{{ isUpdate ? 'Update Contact' : 'Add Contact' }}</h2>
    <form @submit.prevent="handleSubmit">
      <input v-model="formInput.name" type="text" placeholder="Name" />
      <input v-model="formInput.phone" type="text" placeholder="Phone" />
      <button type="submit">{{ isUpdate ? 'Update' : 'Add' }}</button>
    </form>
    <ul>
      <li v-for="contact in dataContacts" :key="contact.id">
        <itemContact :contact="contact" :editContact="editContact" :deleteContact="deleteContact"/>
      </li>
    </ul>
  </div>
</template>

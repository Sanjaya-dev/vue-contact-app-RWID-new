<script setup lang="ts">
    defineProps(['contact', 'editContact', 'deleteContact'])
</script>

    <template>
        <div>
            <p>{{ contact.name }}</p>
            <p>{{ contact.phone }}</p>
            <button @click="editContact(contact.id)">Edit</button>
            <button @click="deleteContact(contact.id)">Delete</button>
        </div>
    </template>
    


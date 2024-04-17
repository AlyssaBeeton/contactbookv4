<template>
  <Header />
  <div class="container mt-3">
    <h1 class="mb-4">Your Contacts</h1>
    <button class="btn btn-primary mb-3" @click="toggleAddForm">Add Contact</button>
    <!-- Search bar -->
    <input type="text" v-model="searchQuery" class="form-control mb-3" placeholder="Search contacts...">
    
    <!-- Add Contact form -->
    <div v-if="showAddForm" class="card mb-3">
      <div class="card-body">
        <h2 class="card-title">Add Contact</h2>
        <ContactForm :contacts="contacts" />
      </div>
    </div>

    <!-- Edit Contact form -->
    <div v-if="showEditForm" class="card mb-3">
      <div class="card-body">
        <h2 class="card-title">Edit Contact</h2>
        <ContactForm :initialContact="selectedContact" :contacts="contacts" />
      </div>
    </div>
    
    <!-- Contact details -->
    <div v-if="selectedContact && selectedContact === activeContact" class="card mt-3">
      <div class="card-body">
        <h2 class="card-title">Contact Details</h2>
        <p><strong>First Name:</strong> {{ selectedContact.firstName }}</p>
        <p><strong>Last Name:</strong> {{ selectedContact.lastName }}</p>
        <p><strong>Email:</strong> {{ selectedContact.email }}</p>
        <button @click="toggleEditForm" class="btn btn-primary">Edit Contact</button>
      </div>
    </div>
    
    <!-- Contact list -->
    <ul class="list-group mt-3">
      <li v-for="contact in sortedFilteredContacts" :key="contact.id" class="list-group-item">
        <div @click="toggleContactDetails(contact)" class="contact-name">{{ contact.firstName }} {{ contact.lastName }}</div>

        <!-- Add a delete button -->
        <button @click.stop="deleteContact(contact)" class="btn btn-danger btn-sm">Delete</button>
      </li>
    </ul>
  </div>
</template>


<script>
import { ref, computed } from 'vue';
import Header from '@/components/Header.vue';
import ContactForm from '@/components/ContactForm.vue';

export default {
  components: {
    Header,
    ContactForm,
  },
  setup() {
    const showAddForm = ref(false);
    const showEditForm = ref(false);
    const selectedContact = ref(null);
    const activeContact = ref(null); // New reference for the currently active contact
    const searchQuery = ref('');
    
    const toggleAddForm = () => {
      showAddForm.value = !showAddForm.value;
      showEditForm.value = false;
    };

    const toggleEditForm = () => {
      showEditForm.value = !showEditForm.value;
      showAddForm.value = false;
    };
    
    const toggleContactDetails = (contact) => {
      if (selectedContact.value === contact) {
        // If the same contact is clicked again, close the details
        activeContact.value = null;
        selectedContact.value = null;
      } else {
        selectedContact.value = contact;
        activeContact.value = contact;
        showAddForm.value = false;
        showEditForm.value = false;
      }
    };

    const deleteContact = (contact) => {
      const index = contacts.value.findIndex(c => c.id === contact.id);
      if (index !== -1) {
        contacts.value.splice(index, 1);
        localStorage.setItem('contacts', JSON.stringify(contacts.value));
      }
    };

    const handleContactAdded = () => {
      showAddForm.value = false; // Close the add form after a contact is added
    };

    const contacts = ref([]);
    const storedContacts = localStorage.getItem('contacts');
    if (storedContacts) {
      contacts.value = JSON.parse(storedContacts);
    } else {
      console.error('No contacts found in local storage.');
    }

    // Computed property to filter contacts based on search query
    const filteredContacts = computed(() => {
      return contacts.value.filter(contact =>
        contact.firstName.toLowerCase().includes(searchQuery.value.toLowerCase()) ||
        contact.lastName.toLowerCase().includes(searchQuery.value.toLowerCase())
      );
    });

    // Computed property to sort filtered contacts by last name
    const sortedFilteredContacts = computed(() => {
      return filteredContacts.value.slice().sort((a, b) => {
        const lastNameA = a.lastName.toLowerCase();
        const lastNameB = b.lastName.toLowerCase();
        if (lastNameA < lastNameB) return -1;
        if (lastNameA > lastNameB) return 1;
        return 0;
      });
    });

    return {
      showAddForm,
      showEditForm,
      selectedContact,
      toggleAddForm,
      toggleEditForm,
      toggleContactDetails,
      deleteContact,
      searchQuery,
      contacts,
      filteredContacts,
      activeContact,
      sortedFilteredContacts,
      handleContactAdded,
    };
  },
};
</script>

<style>

</style>

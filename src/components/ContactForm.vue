<template>
  <form @submit.prevent="handleSubmit">
    <div class="mb-3">
      <label for="firstName" class="form-label">First Name</label>
      <input type="text" v-model="form.firstName" id="firstName" class="form-control" required>
    </div>
    <div class="mb-3">
      <label for="lastName" class="form-label">Last Name</label>
      <input type="text" v-model="form.lastName" id="lastName" class="form-control" required>
    </div>
    <div class="mb-3">
      <label for="email" class="form-label">Email</label>
      <input type="email" v-model="form.email" id="email" class="form-control" required>
    </div>
    <button type="submit" class="btn btn-primary">{{ isEditing ? 'Update Contact' : 'Add Contact' }}</button>
  </form>
</template>

<script>
export default {
  props: {
    initialContact: Object,
    contacts: Array
  },
  data() {
    return {
      form: {
        id: this.initialContact ? this.initialContact.id : null,
        firstName: this.initialContact ? this.initialContact.firstName : '',
        lastName: this.initialContact ? this.initialContact.lastName : '',
        email: this.initialContact ? this.initialContact.email : ''
      },
      isEditing: !!this.initialContact
    };
  },
  methods: {
    handleSubmit() {
      if (this.isEditing) {
        // Update existing contact
        const index = this.contacts.findIndex(contact => contact.id === this.form.id);
        if (index !== -1) {
          this.contacts[index] = { ...this.form };
          localStorage.setItem('contacts', JSON.stringify(this.contacts));
        }
      } else {
        // Add new contact
        this.contacts.push({ ...this.form, id: Date.now() });
        localStorage.setItem('contacts', JSON.stringify(this.contacts));
        this.$emit('contactAdded'); // Emit event when a contact is added
      }
      this.resetForm();
    },
    resetForm() {
      this.form = {
        id: null,
        firstName: '',
        lastName: '',
        email: ''
      };
      this.isEditing = false;
    }
  }
};
</script>

<style>
/* You can add custom CSS here if needed */
</style>

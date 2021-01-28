<template>
  <div id="home">
    <h2>Contacts</h2>
    <p>
      <input v-model="name" placeholder="Name" />
      <input v-model="address" placeholder="Address" />
      <input v-model="phone" placeholder="Phone" />
      <input v-model="email" placeholder="Email" />
      <input v-model="category" placeholder="Category" />
      <button @click="addContact">Add Contact</button>
    </p>
    <div class="contact" v-for="(contact, n) in contacts" :key="contact.id">
      <div v-if="editing === contact.id">
        <input class="editing-input" type="text" v-model="contact.name" />
      </div>
      <span v-else class="property">{{ contact.name }}</span>
      <div v-if="editing === contact.id">
        <input class="editing-input" type="text" v-model="contact.address" />
      </div>
      <span v-else class="property">{{ contact.address }}</span>
      <div v-if="editing === contact.id">
        <input class="editing-input" type="text" v-model="contact.phone" />
      </div>
      <span v-else class="property">{{ contact.phone }}</span>
      <div v-if="editing === contact.id">
        <input class="editing-input" type="text" v-model="contact.email" />
      </div>
      <span v-else class="property">{{ contact.email }}</span>
      <div v-if="editing === contact.id">
        <input class="editing-input" type="text" v-model="contact.category" />
      </div>
      <span v-else class="property">{{ contact.category }}</span>
      <div v-if="editing === contact.id">
        <button @click="editContact(contact.id, contact)">Save</button>
        <button @click="editing = null">Cancel</button>
      </div>
      <div v-else>
        <button @click="removeContact(n)">Remove</button>
        <button @click="editMode(contact.id)">Edit</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
  data() {
    return {
      id: 0,
      name: "",
      address: "",
      phone: "",
      email: "",
      category: "",
      contacts: [],
      newContact: {
        name: "",
        address: "",
        phone: "",
        email: "",
        category: "",
        id: null,
      },
      editing: null,
    };
  },
  mounted() {
    if (localStorage.getItem("contacts")) {
      try {
        this.contacts = JSON.parse(localStorage.getItem("contacts"));
      } catch (e) {
        localStorage.removeItem("contacts");
      }
    }
  },
  methods: {
    addContact() {
      this.newContact = {
        name: this.name,
        address: this.address,
        phone: this.phone,
        email: this.email,
        category: this.category,
        id: this.id++,
      };
      this.contacts.push(this.newContact);
      this.saveContacts();

      this.name = "";
      this.address = "";
      this.phone = "";
      this.email = "";
      this.category = "";

      this.newContact = {
        name: this.name,
        address: this.address,
        phone: this.phone,
        email: this.email,
        category: this.category,
        id: this.id,
      };
    },
    removeContact(x) {
      this.contacts.splice(x, 1);
      this.saveContacts();
    },
    editMode(id) {
      this.editing = id;
    },
    editContact(id, updatedContact) {
      this.contacts = this.contacts.map((contact) =>
        contact.id === id ? updatedContact : contact
      );
      this.editing = null;
      this.saveContacts();
    },
    saveContacts() {
      const parsed = JSON.stringify(this.contacts);
      localStorage.setItem("contacts", parsed);
    },
  },
};
</script>

<style scoped>
.contact {
  display: flex;
  justify-content: flex-start;
  flex-direction: row;
  margin-bottom: 10px;
  margin-left: 15px;
  width: 100%;
}

.property {
  margin: 0 15px;
}
</style>
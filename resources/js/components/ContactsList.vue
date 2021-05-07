<template>
    <div>
        <div v-if="loading">Loading...</div>
        <div v-else>
            <div v-if="contacts.length === 0">
                No contacts yet.
                <router-link to="/contacts/create">Get started</router-link>
            </div>
            <div v-else v-for="contact in contacts" :key="contact">
                <router-link
                    :to="'/contacts/' + contact.data.contact_id"
                    class="flex items-center border-b border-gray-400 p-4 hover:bg-gray-100"
                >
                    <user-circle :name="contact.data.name" />
                    <div class="pl-4">
                        <p class="text-blue-400 font-bold">
                            {{ contact.data.name }}
                        </p>
                        <p class="text-gray-600">{{ contact.data.company }}</p>
                    </div>
                </router-link>
            </div>
        </div>
    </div>
</template>

<script>
import UserCircle from "./UserCircle";
export default {
    name: "ContactsList",
    components: {
        UserCircle
    },
    data: () => ({
        loading: true,
        contacts: null
    }),
    props: ["endpoint"],
    mounted() {
        axios
            .get(this.endpoint)
            .then(response => {
                this.contacts = response.data.data;
                this.loading = false;
            })
            .catch(error => {
                this.loading = false;
                alert("Unable to fetch contacts");
            });
    }
};
</script>

<style scoped></style>

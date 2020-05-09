<template>
    <div>
        <div v-if="loading">Loading...</div>
        <div v-else>
            <div class="flex justify-between">
                <a href="#" class="text-blue-400" @click="$router.back()">
                    < Back
                </a>
                <div class="relative">
                    <router-link :to="'/contacts/' + contact.contact_id + '/edit'" tag="button"
                                 class="px-4 py-2 rounded text-sm text-green-500 border border-green-500 font-bold mr-2">
                        Edit
                    </router-link>
                    <button class="px-4 py-2 border border-red-500 rounded text-sm font-bold text-red-500"
                    @click="modal=!modal">Delete</button>

                    <div v-if="modal" class="absolute bg-blue-900 text-white rounded-lg z-20 p-8 w-64
                     right-0 mt-2 mr-6">
                        <p>Are you sure you want to delete this record?</p>

                        <div class="flex items-center mt-6 justify-end">
                            <button class="text-white pr-4" @click="modal= !modal">Cancel</button>
                            <button class="px-4 py-2 bg-red-500 rounded text-sm font-bold text-white"
                            @click="destroy">Delete</button>
                        </div>
                    </div>

                </div>
                <div v-if="modal" @click="modal=!modal" class="bg-black opacity-25 absolute right-0 left-0 top-0 bottom-0 z-10"></div>
            </div>

            <div class="flex items-center pt-6">
                <user-circle :name="contact.name"/>
                <p class="pl-5 text-xl">{{contact.name}}</p>
            </div>

            <p class="pt-6 text-gray-600 uppercase font-bold text-sm">Email</p>
            <p class="pt-2 text-blue-400">{{contact.email}}</p>
            <p class="pt-6 text-gray-600 uppercase font-bold text-sm">Company</p>
            <p class="pt-2 text-blue-400">{{contact.company}}</p>
            <p class="pt-6 text-gray-600 uppercase font-bold text-sm">Birthday</p>
            <p class="pt-2 text-blue-400">{{contact.birthday}}</p>
        </div>
    </div>
</template>

<script>
    import UserCircle from "../UserCircle";

    export default {
        name: "ContactsShow",
        components: {
            UserCircle
        },
        data: () => ({
            contact: null,
            loading: true,
            modal: false
        }),
        mounted() {
            axios.get('/api/contacts/' + this.$route.params.id)
                .then(response => {
                    this.contact = response.data.data;
                    this.loading = false;
                })
                .catch(errors => {
                    this.loading = false;
                    if(error.response.status === '404') {
                        this.$router.push('/contacts');
                    }
                })
        },
        methods: {
            destroy: function() {
                axios.delete('/api/contacts/' + this.$route.params.id)
                .then(response => {
                    this.$router.push('/contacts');
                })
                .catch(errors => {
                    alert('Internal Erorr. Unable to delete.');
                })
            }
        }
    }
</script>

<style scoped>

</style>

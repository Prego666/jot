<template>
    <div>
        <div class="flex justify-between">
            <a href="#" class="text-blue-400" @click="$router.back()">
                < Back
            </a>
        </div>
        <form @submit.prevent="submitForm">
            <input-field name="name" label="Contact name" placeholder="Contact Name"
                         @update:field="form.name= $event" :errors="errors" :data="form.name"/>
            <input-field name="email" label="Contact email" placeholder="Contact Email"
                         @update:field="form.email = $event" :errors="errors" :data="form.email"/>
            <input-field name="company" label="Company" placeholder="Company"
                         @update:field="form.company = $event" :errors="errors" :data="form.company"/>
            <input-field name="birthday" label="Birthday" placeholder="MM/DD/YYYY"
                         @update:field="form.birthday = $event" :errors="errors" :data="form.birthday"/>
            <div class="flex justify-end">
                <button class="py-2 px-4 rounded text-red-700 border mr-5 hover:border-red-700">Cancel</button>
                <button class="bg-blue-500 py-2 px-4 text-white rounded hover:bg-blue-400">Save</button>
            </div>
        </form>
    </div>
</template>

<script>
    import InputField from "../InputField";

    export default {
        name: "ContactsEdit",
        components: {
            InputField
        },
        data: () => ({
            form: {
                'name': null,
                'email': null,
                'company': null,
                'birthday': null,
            },
            errors: null
        }),
        mounted() {
            axios.get('/api/contacts/' + this.$route.params.id)
                .then(response => {
                    this.form = response.data.data;
                })
                .catch(error => {
                    this.loading = false;
                    if(error.response.status === '404') {
                        this.$router.push('/contacts');
                    }
                })
        },
        methods: {
            submitForm: function () {
                axios.patch('/api/contacts/' + this.$route.params.id, this.form)
                    .then(response => {
                        this.$router.push(response.data.links.self)
                    })
                    .catch(errors => {
                        this.errors = errors.response.data.errors;
                    });
            }
        }
    }
</script>

<style scoped>

</style>

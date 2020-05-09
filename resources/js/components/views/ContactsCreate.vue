<template>
    <div>
        <form @submit.prevent="submitForm">
            <input-field name="name" label="Contact name" placeholder="Contact Name"
                         @update:field="form.name= $event" :errors="errors"/>
            <input-field name="email" label="Contact email" placeholder="Contact Email"
                         @update:field="form.email = $event" :errors="errors"/>
            <input-field name="company" label="Company" placeholder="Company"
                         @update:field="form.company = $event" :errors="errors"/>
            <input-field name="birthday" label="Birthday" placeholder="MM/DD/YYYY"
                         @update:field="form.birthday = $event" :errors="errors"/>
            <div class="flex justify-end">
                <button @click.prevent="$router.back()" class="py-2 px-4 rounded text-red-700 border mr-5 hover:border-red-700">Cancel</button>
                <button class="bg-blue-500 py-2 px-4 text-white rounded hover:bg-blue-400">Add new contact</button>
            </div>
        </form>
    </div>
</template>

<script>
    import InputField from "../InputField";

    export default {
        name: "ContactsCreate",
        components: {
            InputField
        },
        data: () => ({
            form: {
                'name': '',
                'email': '',
                'company': '',
                'birthday': '',
            },
            errors: null
        }),
        methods: {
            submitForm: function () {
                axios.post('/api/contacts', this.form)
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

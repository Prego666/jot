<template>
        <contacts-list endpoint="/api/contacts"/>
</template>

<script>
    import ContactsList from "../ContactsList";
    export default {
        name: "ContactsIndex",
        components: {
            ContactsList
        }
    }
</script>

<style scoped>

</style>

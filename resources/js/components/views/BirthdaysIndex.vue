<template>
        <contacts-list endpoint="/api/birthdays"/>
</template>

<script>
    import ContactsList from "../ContactsList";
    export default {
        name: "BirthdaysIndex",
        components: {
            ContactsList
        }
    }
</script>

<style scoped>

</style>

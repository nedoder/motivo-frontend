<template>
    <CCol sm="12">
        <CCard bodyWrapper text-color="blue" :style="{color: activeColor}">
            <CRow>
                <CCol sm="10">
                    {{text}}
                </CCol>
                <CCol sm="2">
                    {{number}}
                </CCol>
            </CRow>
        </CCard>
    </CCol>
</template>

<script>
    export default {
        name: 'Title',
        props: {
            activeColor: String,
            text: String,
            number: Object
        }
    }
</script>

<style>

</style>
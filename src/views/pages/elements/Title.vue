<template>
    
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
    
</template>

<script>
    export default {
        name: 'Title',
        props: {
            activeColor: String,
            text: String,
            number: Number
        }
    }
</script>

<style>

</style>
<template>
    <CRow>
        <CCardBody>
            <Title text="To do" :number="toDo" />
        </CCardBody>
        <CCardBody>
            <Title text="In progess" :number="failed" />
        </CCardBody>
        <CCardBody>
            <Title text="Done" :number="passed" />
        </CCardBody>
        ffdfdfdf
    </CRow>
</template>

<script>
    import Title from './elements/Title.vue';

    export default {
        name: 'Tasks',
        components: {
            Title
        },
        data() {
            return {
                toDo: 3,
                failed: 5,
                passed: 6
            }
        },
        //   watch: {
        //     $route: {
        //       immediate: true,
        //       handler (route) {
        //         if (route.query && route.query.page) {
        //           this.activePage = Number(route.query.page)
        //         }
        //       }
        //     }
        //   },
        //   methods: {
        //     getBadge (status) {
        //       switch (status) {
        //         case 'Active': return 'success'
        //         case 'Inactive': return 'secondary'
        //         case 'Pending': return 'warning'
        //         case 'Banned': return 'danger'
        //         default: 'primary'
        //       }
        //     },
        //     rowClicked (item, index) {
        //       this.$router.push({path: `users/${index + 1}`})
        //     },
        //     pageChange (val) {
        //       this.$router.push({ query: { page: val }})
        //     }
        //   }
    }
</script>
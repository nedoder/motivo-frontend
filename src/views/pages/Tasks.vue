<template>
    <div>
        <CRow>
            <TheHeader/>
        </CRow>
        <CRow>
            <CCardBody color="white">
                <CCol sm="12">
                    <CProgress :value="counter" color="success" animated show-value style="height:40px;" class="mt-1" />
                </CCol>
            </CCardBody>
        </CRow>
        <CRow>
            <CCardBody>
                <Title text="To do" :number="toDo" activeColor="blue" />
                <CCol sm="12">
                    <CJumbotron v-for="challenge in challenges">
                        <h1 class="display-3">{{challenge.title}}</h1>
                        <p class="lead">Coins to win: {{challenge.coins_to_win}}</p>
                        <CButton color="primary" href="https://coreui.io/" target="_blank">More Info</CButton>
                    </CJumbotron>
                </CCol>
            </CCardBody>
            <CCardBody>
                <Title text="Attempt" :number="failed" activeColor="red" />
                 <CCol sm="12">
                    <CJumbotron v-for="attempt in attempts">
                        <h1 class="display-3">{{attempt.challenge.title}}</h1>
                        <p class="lead">Coins to win: {{attempt.challenge.id}}</p>
                        <!-- <CButton color="primary" href="https://coreui.io/" target="_blank">More Info</CButton> -->
                    </CJumbotron>
                </CCol>
            </CCardBody>
            <CCardBody>
                <Title text="Passed" :number="passed" activeColor="green" />
                <CCol sm="12">
                    <CJumbotron v-for="complet in complets">
                        <h1 class="display-3">{{complet.challenge.title}}</h1>
                        <p class="lead">Coins to win: </p>
                        <CButton color="primary" href="https://coreui.io/" target="_blank">More Info</CButton>
                    </CJumbotron>
                </CCol>
            </CCardBody>
        </CRow>
    </div>
</template>

<script>
    import Title from './elements/Title.vue';
    import TheHeader from '../../containers/TheHeader.vue'

    export default {
        name: 'Tasks',
        components: {
            Title,
            TheHeader
        },
        data() {
            return {
                challenges: [],
                attempts: [],
                complets: [],
                toDo: 3,
                failed: 5,
                passed: 6,
                counter: 65,
            }
        },
        mounted() {
    const token = JSON.parse(JSON.stringify(localStorage.getItem('user-token')))
    const bearer = 'Bearer ' + token
    const challenges = axios({
        method:'get',
        url: '/api/challenges/',
      headers: { 'Authorization': bearer, }
    });
    //    .then(resp=>{
    //        // this.title = resp.data.results[0].title
    //       // this.price = resp.data.results[0].price
    //       // this.image = resp.data.results[0].image
    //    })
      

    const attempts = axios({
        method:'get',
        url: '/api/attempt/',
      headers: { 'Authorization': bearer, }
    });

    const complets = axios({
        method:'get',
        url: '/api/completed/',
      headers: { 'Authorization': bearer, }
    });

    Promise.all([challenges, attempts, complets]).then(([chal, att, com]) => {
        console.log(chal, att);
        this.attempts = att.data.results;
        this.challenges = chal.data.results;
        this.complets = com.data.results;
    }).catch(error => console.log(error))
    //    .then(resp=>{

    //       // this.title = resp.data.results[0].title
    //       // this.price = resp.data.results[0].price
    //       // this.image = resp.data.results[0].image
    //    })
    //   .catch(error => console.log(error))
//   })

      
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
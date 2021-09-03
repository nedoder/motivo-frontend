<template>
    <div>
        <CRow>
            <CCardBody color="white" class="mb-4">
                <CCol sm="12">
                    <h1 class="text-center font-weight-bold">Progress</h1>
                    <CProgress :value="countRate" color="success" animated show-value style="height:40px;" />
                </CCol>
            </CCardBody>
        </CRow>
        <CRow>
            <CCol sm='4'>
                <Title text="To do" :number="toDo" activeColor="blue" />
                <CJumbotron v-for="challenge in challenges" @click="taskClicked(challenge)" class='bg-warning'>
                    <h3>{{challenge.title}}</h3>
                    <p class="lead">Coins to win: {{challenge.coins_to_win}}</p>
                    <CButton color="danger" size='lg' target="_blank">Click me</CButton>
                </CJumbotron>
            </CCol>
            <CCol sm='4'>
                <Title text="Attempt" :number="failed" activeColor="red" />
                <CJumbotron v-for="attempt in attempts" class='bg-secondary'>
                    <h1>{{attempt.challenge.title}}</h1>
                    <p class="lead">Coins to win: {{attempt.user.username}}</p>
                </CJumbotron>
            </CCol>
            <CCol sm='4'>
                <Title text="Passed" :number="passed" activeColor="green" />
                <CJumbotron v-for="complet in complets" class='bg-secondary'>
                    <h1>{{complet.challenge.title}}</h1>
                    <p class="lead">Coins collected: {{complet.challenge.id}}</p>
                </CJumbotron>
            </CCol>
        </CRow>
    </div>
</template>

<script>
    import Title from './elements/Title.vue';
    import TheHeader from '../../containers/TheHeader.vue'
    import axios from 'axios'

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
                toDo: null,
                failed: null,
                passed: null,
                counter: null,
            }
        },
        mounted() {
            const token = JSON.parse(JSON.stringify(localStorage.getItem('user-token')))
            const bearer = 'Bearer ' + token
            const challenges = axios({
                method: 'get',
                url: '/api/challenges/',
                headers: {
                    'Authorization': bearer,
                }
            });

            const attempts = axios({
                method: 'get',
                url: '/api/attempt/',
                headers: {
                    'Authorization': bearer,
                }
            });

            const complets = axios({
                method: 'get',
                url: '/api/completed/',
                headers: {
                    'Authorization': bearer,
                }
            });

            Promise.all([challenges, attempts, complets]).then(([chal, att, com]) => {
                console.log(chal, att);
                this.attempts = att.data.results;
                this.failed = att.data.results.length;
                this.challenges = chal.data.results;
                this.toDo = chal.data.results.length;
                this.complets = com.data.results;
                this.passed = com.data.results.length;
            }).catch(error => console.log(error))
        },
        watch: {
            $route: {
                immediate: true,
                handler(route) {
                    if (route.query && route.query.page) {
                        this.activePage = Number(route.query.page)
                    }
                }
            }
        },
        methods: {
            //     getBadge (status) {
            //       switch (status) {
            //         case 'Active': return 'success'
            //         case 'Inactive': return 'secondary'
            //         case 'Pending': return 'warning'
            //         case 'Banned': return 'danger'
            //         default: 'primary'
            //       }
            //     },
            taskClicked(challenge) {
                this.$router.push({
                    path: `tasks/${challenge.id}`
                })
            },
    
            //     pageChange (val) {
            //       this.$router.push({ query: { page: val }})
            //     }
        },
        computed: {
            countRate: function() {
                return Math.round(this.toDo/(this.failed+this.toDo+this.passed)*100)}
        }
    }
</script>
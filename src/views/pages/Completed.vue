<template>
  <CRow class="d-flex justify-content-center">
    <CCol col="12" lg="6">
      <CCard>
        <CCardHeader class='bg-danger text-white'>
          <p>Mandatory</p>
          <h2>Challenge Id: {{ $route.params.id }}</h2>
        </CCardHeader>
        <CCardBody>
          <CRow>
            <CButton disabled color="secondary" variant="pill" size="lg">To do
            </CButton>
            <CButton disabled color="secondary" variant="pill" size="lg">In progress
            </CButton>
            <CButton disabled color="info" variant="pill" size="lg">Done
            </CButton>
          </CRow>
          <CRow class='mt-3'>
            <p>Coins to win:<span>{{this.tasks.coins_to_win}}</span></p>
          </CRow>
          <CRow>
            <p>Description:<span>{{this.tasks.description}}</span></p>
          </CRow>
        </CCardBody>
        <CCardFooter class="d-flex justify-content-center">
          <CButton class="col-3" color="danger" size='lg' @click="goBack">Back</CButton>
        </CCardFooter>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Completed',
    beforeRouteEnter(to, from, next) {
      next(vm => {
        vm.challengesOpened = from.fullPath.includes('tasks')
      })
    },
    data() {
      return {
        usersOpened: null,
        tasks: null
      }
    },
    computed: {
      challengeData() {
        const id = this.$route.params.id
        const task = this.challenges.find((task, index) => index + 1 == id)
        const taskDetails = task ? Object.entries(task) : [
          ['id', 'Not found']
        ]
        return taskDetails.map(([key, value]) => {
          return {
            key,
            value
          }
        })
      },
    },
    methods: {
      goBack() {
        this.$router.push({
          path: '/dashboard/tasks'
        })
      }
    },
    mounted() {
      const token = localStorage.getItem('user-token')
      const bearer = 'Bearer ' + token
      const tasks = axios({
        method: 'get',
        url: `https://api.motivo.localhost/challenges/${this.$route.params.id}`,
        headers: {
          'Authorization': bearer,
        }
      });

      Promise.all([tasks]).then(([chal]) => {
        console.log(chal);
        this.tasks = chal.data;
        console.log(this.tasks)

      }).catch(error => console.log(error))
//POST https://api.motivo.localhost/attempt/
      // {
//       "user": {
//         "username": "nedoder1",
//         "id": 1
//       },
//       "challenge": {
//         "title": "test",
//         "id": 1,
//         "coins": 3,
//         "description": "ddjjd"
//       },
//       "file": null,
//       "confirmed_by_admin": false
//     },
    },
  }
</script>


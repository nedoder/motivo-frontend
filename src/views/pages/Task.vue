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
            <CButton color="secondary" variant="pill" size="lg">To do
            </CButton>
            <CButton color="info" variant="pill" size="lg">In progress
            </CButton>
            <CButton color="secondary" variant="pill" size="lg">Done
            </CButton>
          </CRow>
          <CRow class='mt-3'>
            <p>Coins to win:<span>45</span></p>
          </CRow>
          <CRow>
            <p>Description:<span>blablablablabla</span></p>
          </CRow>
          <CRow>
            <CInputFile label="File input" horizontal invalid-feedback="Please provide a required input." />
          </CRow>
        </CCardBody>
        <CCardFooter>
          <CButton color="danger" size='lg' @click="goBack">Back</CButton>
        </CCardFooter>
      </CCard>
    </CCol>
  </CRow>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'Task',
    beforeRouteEnter(to, from, next) {
      next(vm => {
        vm.challengesOpened = from.fullPath.includes('tasks')
      })
    },
    data() {
      return {
        usersOpened: null,
        tasks: []
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
      visibleData() {
        return this.tasks.filter(param => param.key !== 'id')
      },
      username() {
        return this.tasks.filter(param => param.key === 'id')[0].value
      }
    },
    methods: {
      goBack() {
        this.$router.push({
          path: '/dashboard/tasks'
        })
      }
    },
    mounted() {
      const token = JSON.parse(JSON.stringify(localStorage.getItem('user-token')))
      const bearer = 'Bearer ' + token
      const tasks = axios({
        method: 'get',
        url: '/api/challenges/',
        headers: {
          'Authorization': bearer,
        }
      });

      Promise.all([tasks]).then(([chal]) => {
        console.log(chal);
        this.tasks = chal.data.results;
        console.log(this.tasks)

      }).catch(error => console.log(error))
    },
  }
</script>
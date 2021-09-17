<template>
  <CFooter :fixed="false">
    <div>
      <a href="https://vstorm.co/" target="_blank">
      <span class="ml-1">&copy; {{new Date().getFullYear()}} VSTORM All right reserved.</span>
      </a>
    </div>
    <div class="mfs-auto">
      <span class="mr-1" target="_blank"></span>
      <CIcon name="cib-linkedin"/><a href="https://www.linkedin.com/company/vstormco/"></a>
      <CIcon name="cib-facebook"/><a href="https://www.facebook.com/vstormco/"></a>
      <CIcon name="cib-dribbble"/><a href="https://dribbble.com/vstorm"></a>
    </div>
  </CFooter>
</template>

<script>
export default {
  name: 'TheFooter'
}
</script>

<template>
<ClientForm></ClientForm>
</template>

<script>
import ClientForm from './components/ClientForm.vue'

export default {
  components: { ClientForm },
  name: "App",
 data() {
    return { 
        
    }
}
}
</script>

<style lang="sass">
@import './assets/sass/_variables'

* 
  transition: .5s ease-in-out
  margin: 0
  padding: 0

</style>

<template>
  <div class="module">
    <h2><i class="fa fa-globe"></i> Domain availability check</h2>
    <span class="info">Results may be cached for up to an hour.<br><small>This check tests if a SOA record exists for the specific domains. For domains that are invalid (in terms of forbidden names or lengths), the result may be wrong.</small></span>
    <p v-if="status === 0">Press ENTER in the inputbox to run this check.</p>
    <p v-if="status === 1"><i class="fa fa-pulse fa-spinner"></i> Checking .org, .net, .com, .io, .de domain ...</p>
    <div v-if="status === 2">
      <p v-for="domain in availability" :key="domain.domain">
        <b v-if="domain.available" class="success"><i class="fa fa-check-circle"></i> The domain <span class="domain">{{ domain.domain }}</span> is (probably) available.</b>
        <b v-if="!domain.available" class="failure"><i class="fa fa-times-circle"></i> The domain <a :href="'http://' + domain.domain" class="domain" target="_blank">{{ domain.domain }}</a> is (probably) not available.</b>
      </p>
    </div>
  </div>
</template>

<script>
import { fetch } from '../fetchTimeout'

export default {
  name: 'DomainCheck',
  data () {
    return {
      status: 0,
      availability: []
    }
  },
  methods: {
    runChecks (name) {
      this.status = 1
      this.availability = []
      const urlName = encodeURIComponent(name).toLowerCase()
      Promise.allSettled([
        fetch('https://domain-available.liltv.media/?domain=' + urlName + '.net').then(resp => resp.json()),
        fetch('https://domain-available.liltv.media/?domain=' + urlName + '.org').then(resp => resp.json()),
        fetch('https://domain-available.liltv.media/?domain=' + urlName + '.com').then(resp => resp.json()),
        fetch('https://domain-available.liltv.media/?domain=' + urlName + '.de').then(resp => resp.json()),
        fetch('https://domain-available.liltv.media/?domain=' + urlName + '.io').then(resp => resp.json())
      ]).then(results => {
        results.forEach(result => {
          if (result.status === 'fulfilled') {
            const domain = result.value
            if (Object.prototype.hasOwnProperty.call(domain, 'success') && domain.success) {
              this.availability.push(domain)
            }
          }
        })
        this.status = 2
      }).catch(err => console.error(err))
    }
  }
}
</script>

<style scoped>
.domain {
  color: #75ffef;
}
.info {
  color: #a5a5a5;
}
</style>

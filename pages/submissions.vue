<template>
  <div class="container">
    <h1>Submissions</h1>
    <LoginPrompt />
    <br>
    <table>
      <tr>
        <th>
          id
        </th>
        <th>name</th>
        <th>submitter</th>
      </tr>
      <tr v-for="d in prList" :key="d.number">
        <td>{{ d.number }}</td>
        <td>
          <nuxt-link :to="`/submission/${d.number}`">
            <b>{{ d.title }}</b>
          </nuxt-link>
        </td>
        <td>
          <a :href="d.user.html_url">
            {{ d.user.login }}
          </a>
        </td>
      </tr>
    </table>
  </div>
</template>

<script>
// import Base64 from '~/assets/base64'

const PR = '/repos/EtherOJ/submissions/pulls?state=all'

export default {
  auth: false,
  data () {
    return {
      prList: []
    }
  },
  async mounted () {
    this.prList = await this.$axios.$get(PR)
    this.$store.dispatch('problems/fetch')
  },
  methods: {
    getProblemName (ref) {
      const o = this.$store.state.problems.problemObject[ref]
      return o ? o.name : 'Unknown Problem'
    }
  }
}

</script>

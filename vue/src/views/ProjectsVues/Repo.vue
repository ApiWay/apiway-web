<template>
            <div class="card">
              <div class="card-block">
                <img>
            <!--<h4 class="card-title">Github</h4>-->
            <!--<p class="card-text">...</p>-->
              </div>
          <ul class="list-group list-group-flush">
            <li class="list-group-item justify-content-between" v-for="repo in repos">
              {{ repo.full_name }}
              <button type="button" @click="runTest(repo)" class="btn btn-info">Run</button>
            </li>
          </ul>
        </div>
 </template>


<script>

var oauth = require('../../util/oauth')

export default {
  name: 'Repo',
  data: () => ({
    loading: true
  }),
  computed: {
    profile () {
      return this.$store.state.profile
    },
    organizations () {
      return this.$store.state.orgs
    },
    repos () {
      return this.$store.state.repos
    },
    token () {
      return oauth.getToken(this.$auth)
    },
    userId () {
      return this.$store.state.userId
    }
  },

  beforeCreate: function () {
    console.log('beforeCreate')
    if (!oauth.isAuthenticated(this.$auth)) {
      console.log('go to Login')
      this.$router.replace('/login')
    } else {

    }
  },
  created: function () {
  },

  asyncData ({ store }) {
    return store.dispatch('FETCH_ORGS', { token: this.token })
  },

  title () {
    return 'RepoInfo'
  },

  beforeMount () {
    this.fetchProfile()
    this.fetchOrgs()
  },

  mounted: function () {
    console.log('mounted')
  },

  watch: {
    organizations: 'fetchOrgs',
    profile: 'fetchProfile',
    userId: 'fetchInstancesByUser'
  },

  methods: {
    fetchOrgs () {
      this.loading = true
      fetchOrgs(this.$store, this.token).then(() => {
        this.loading = false
      })
    },
    fetchProfile () {
      this.loading = true
      console.log(this.$store.state.profile)
      fetchProfile(this.$store, this.token).then(() => {
        this.loading = false
      })
    },
    fetchRepos () {
      this.loading = true
      console.log(this.$store.state.repos)
      fetchRepos(this.$store, this.token).then(() => {
        this.loading = false
      })
    },
    fetchOrgRepos (org) {
      this.loading = true
      console.log(this.$store.state.repos)
      fetchOrgRepos(this.$store, org, this.token).then(() => {
        this.loading = false
      })
    },
    runTest (repo) {
      console.log('runTest: ' + repo)
      addProject(this.$store, repo)
        .then((project) => {
          this.loading = false
//        this.$store.dispatch('SET_ACTIVE_PROJECT', { project: project })
          this.$router.replace(`/dashboard/${repo.full_name}`)
        })
    }
  }
}

function fetchProfile (store, token) {
  if (token) {
    return store.dispatch('FETCH_USER_PROFILE', {
      token: token
    }).then(() => {
      console.log('done FETCH_USER_PROFILE in Repo.vue')
      store.dispatch('UPDATE_USER_PROFILE', {})
        .then(() => {
        }
      )
    })
  }
}

function fetchOrgs (store, token) {
  if (token) {
    return store.dispatch('FETCH_ORGS', {
      token: token
    }).then(() => {
      console.log('done FETCH_ORGS in Repo.vue')
    })
  }
}

function fetchRepos (store, token) {
  if (token) {
    return store.dispatch('FETCH_REPOS', {
      token: token
    }).then(() => {
      console.log('done FETCH_REPOSin Repo.vue')
    })
  }
}

function fetchOrgRepos (store, org, token) {
  if (token) {
    return store.dispatch('FETCH_ORG_REPOS', {
      org: org,
      token: token
    }).then(() => {
      console.log('done FETCH_ORG_REPOS in Repo.vue')
    })
  }
}

function addProject (store, repo) {
  return store.dispatch('ADD_PROJECT', {
    repo: repo
  }).then(() => addInstance(store, store.state.activeProjectId))
    .then(() => {
      console.log('done ADD_PROJECT in Repo.vue')
    })
}

function addInstance (store, projectId) {
  return store.dispatch('ADD_INSTANCE', {
    projectId: projectId
  }).then(() => {
    console.log('done ADD_INSTANCE in Repo.vue')
  })
}
</script>


<style scoped>
  .avatar {
    border-radius: 50px;
  }
  .card {
    border-radius: 3px;
    box-shadow: 0 0 10px 0 rgba(53, 58, 73, 0.20); }


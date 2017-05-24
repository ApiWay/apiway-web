<template>
    <div class="row">
        <div>
             <router-link :to="{ name: 'DashboardAdd'}">Add Instance</router-link>
        </div>
        <div class="col-md-4" style="min-width: 20rem;">
            <cvsInfo/>
        </div>
        <div class="col-md-8">
            <repoInfo/>
        </div>
    </div>
</template>

<script>
import cvsInfo from '../views/ProjectsVues/Cvs'
import repoInfo from '../views/ProjectsVues/Repo'

export default {
  name: 'projects',
  components: {
    cvsInfo,
    repoInfo
  }
}
</script>


<style scoped>
/*  .avatar {
    border-radius: 50px;
  }
  .card {
    border-radius: 3px;
    box-shadow: 0 0 10px 0 rgba(53, 58, 73, 0.20); }*/


</style>

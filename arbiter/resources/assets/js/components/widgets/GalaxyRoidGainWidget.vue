<template>
    <div class="card card-default has-table">
        <div class="card-header">Biggest roid gains</div>

        <div class="card-body">
            <preloader :loading.sync="loading"></preloader>
            <table id="members" class="table table-striped table-bordered" style="width:100%" v-if="!loading">
              <thead>
                  <tr>
                    <th class="text-center">#</th>
                    <th class="align-bottom text-right">x:y</th>
                    <th class="align-bottom text-right">Name</th>
                    <th class="align-bottom text-right">Size</th>
                  </tr>
              </thead>
              <tbody>
                  <tr v-for="(galaxy, index) in pager.data" v-bind:class="{ is_alliance: galaxy.is_alliance }">
                      <td class="text-right">{{ galaxy.rank_size }} <span v-if="galaxy.growth_rank_size > 0" v-tooltip:top="'up ' + galaxy.growth_rank_size + ' places'"><i class="fa fa-caret-up"></i></span><span v-if="galaxy.growth_rank_size < 0" v-tooltip:top="'down ' + Math.abs(galaxy.growth_rank_size) + ' places'"><i class="fa fa-caret-down"></i></span><span v-if="galaxy.growth_rank_size == 0"><i class="fa fa-caret-right"></i></span></td>
                      <td class="text-right"><router-link :to="{ name: 'galaxy', params: { id: galaxy.id }}"><a>{{ galaxy.x }}:{{ galaxy.y }}</a></router-link></td>
                      <td style="width: 100%; white-space: normal">{{ galaxy.name }}</td>
                      <td class="text-right">{{ galaxy.size | shorten }} (<span v-bind:class="{ yellow: galaxy.growth_size == 0, green: galaxy.growth_size > 0, red: galaxy.growth_size < 0 }">{{ galaxy.growth_size | shorten }}</span>)</td>
                  </tr>
              </tbody>
            </table>
        </div>
    </div>
</template>

<script>
    export default {
      data: function () {
          return {
              pager: {},
              sort: '-growth_size',
              loading: true
          };
      },
      methods: {
          loadGalaxies: function() {
              axios.get('/api/v1/galaxies', {
                  params: {
                      sort: this.sort,
                      page: 1,
                      perPage: 5,
                      exclude_c200: true
                  }
              })
              .then((response) => {
                  this.pager = response.data;
                  this.loading = false;
              });
          }
      },
      mounted() {
          this.loadGalaxies();
      }
    }
</script>

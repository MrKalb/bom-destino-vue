<template>
  <card>
    <h4 slot="header" class="card-title">Consulta de pendencias</h4>
    <form>
        <div class="row">
          <div class="col-md-9">
              <select v-model="selectedQuery" class="form-select form-control-lg">
                  <option v-for="option in this.permissions">
                      {{option.toUpperCase()}}
                  </option>
              </select>    
          </div>
      </div>  
      <div class="row" v-show="selectedQuery">
          <div class="col-md-9">
              <select v-model="request.dateToRequest" class="form-select form-control-lg">
                  <option v-for="option in this.dates" >
                      {{option.date}}
                  </option>
              </select>    
          </div>
      </div>
      <div class="row">  
      <div class="col-md-4 text-left">
        <button type="submit" class="btn btn-info btn-fill float-right" @click.prevent="sendRequest">
          Pesquisar
        </button>
      </div>
      </div>
      <div class="clearfix"></div>
    </form>
    <div class="content" v-show="showResponse">
        <div class="container-fluid">
            <div class="row">
                <div class="col-12">
                    <card class="strpied-tabled-with-hover"
                        body-classes="table-full-width table-responsive">
                        <template slot="header">
                            <h4 class="card-title">Pendencias do ano de {{request.dateToRequest}}</h4>
                         </template>
                        <l-table class="table-hover table-striped"
                            :columns="tableColumns"
                            :data="response">
                         </l-table>
                    </card>
                </div>
            </div>
        </div>
    </div>
  </card>
</template>

<script>
import axios from 'axios';
import Card from 'src/components/Cards/Card.vue'
import LTable from 'src/components/Table.vue'
export default {
    components: {
      Card,
      LTable
    },
    data() {
      return {
         request: {
             dateToRequest:'',
             id: ''
         }, 
         dates: [{date: '2020'},{date:'2019'}, {date:'2018'}], 
         permissions: [], 
         selectedQuery: '',
         tableColumns: ['Nome','Tamanho do terreno','Valor','Vencimento', 'Link'],
         showResponse: false, 
         response: []
      }
    },
        methods: {
        checkCredentials() {
            let token = localStorage.getItem('vue-token');
            let base64Url = token.split('.')[1];
            let base64 = base64Url.replace(/-/g, '+').replace(/_/g, '/');
            let jsonPayload = decodeURIComponent(atob(base64).split('').map(function(c) {
            return '%' + ('00' + c.charCodeAt(0).toString(16)).slice(-2);
            }).join(''));

            let decoded = JSON.parse(jsonPayload);
            console.log(decoded);
            let roles = decoded.resource_access['springboot-application'].roles; 
            roles.forEach(role => {
                if(role === 'cpf' || role === 'cnpj') {
                    if(!this.permissions.includes('iptu')) {
                        this.permissions.push('iptu');
                    } else if (roles === 'itr') {
                        this.permissions.push('itr');
                    }
                    this.request.id = decoded.preferred_username; 
                }
            });
        }, 
        sendRequest() {
            let axiosConfig = {
                headers: {
                    Authorization: `Bearer ${localStorage.getItem('vue-token')}`
                }
            }
            let url = `http://localhost:8000/cidadao/${this.selectedQuery.toLowerCase()}`;
            axios.post(url, this.request, axiosConfig).then(response => {
                console.log(response);
                this.response = response.data; 
                this.showResponse = true; 
                console.log(this.response);
            }).catch(err => {
                console.log(err)
            }); 
        }   
    }, 
    mounted () {
        this.checkCredentials()
        
    } 
}
</script>

<style>

</style>
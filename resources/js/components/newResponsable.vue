<template>
    <main class="app-content">
      <div class="app-title">
          <h1><i class="bi bi-person-add"></i> Nuevo Responsable</h1>
      </div>
      <div class="row justify-content-center">
        <div class="col-lg-10">
            <div class="card shadow-lg border-0 rounded-lg mt-12">
              <div class="card-header"><h3 class="text-center font-weight-light my-4"> Oficina: </h3></div>
              <div class="card-body">
                <div class="row">
                  <div class="mb-3 col-md-6">
                    <div class="mb-3 input-group" >
                        <div class="input-group-prepend">
                            <span class="input-group-text">
                              <i class="bi bi-upc"></i>
                            </span>
                        </div>
                        <input type="text" class="form-control"  placeholder="Ingrese Código de Oficina" v-model="codofic" @keyup.enter="buscarOficina()" >
                    </div>
                  </div>
                  <div class="mb-3 col-md-3 form-group">
                    <button class="btn btn-info form-control btnagregar" type="button" @click="buscarOficina()"><i class="bi bi-plus-lg" ></i> Agregar</button>
                  </div>
                  <div class="mb-3 col-md-3 form-group">
                    <button class="btn btn-success form-control btnagregar" type="button" @click="abrirModal()"><i class="bi bi-search"></i> Buscar</button>
                  </div>
                </div>

                <div class="mb-3 row">
                  <label class="form-label col-md-4">Oficina:</label>
                  <div class="col-md-8 input-group">
                    <div class="input-group-prepend">
                        <span class="input-group-text">
                          <i class="bi bi-building"></i>
                        </span>
                    </div>
                    <input class="form-control" type="text"  disabled v-model="nomofic">
                  </div>
                </div>

                <div class="mb-3 row">
                  <label class="form-label col-md-4">Nombre del Responsable:</label>
                  <div class="col-md-8 input-group">
                    <div class="input-group-prepend">
                        <span class="input-group-text">
                          <i class="bi bi-file-person"></i>
                        </span>
                    </div>
                    <input class="form-control" type="text" v-model="nomresp">
                  </div>
                </div>

                <div class="mb-3 row">
                  <label class="form-label col-md-4">Cargo:</label>
                  <div class="col-md-8 input-group">
                    <div class="input-group-prepend">
                        <span class="input-group-text">
                          <i class="bi bi-postcard"></i>
                        </span>
                    </div>
                    <input class="form-control" type="text"  v-model="cargo">
                  </div>
                </div>

                <div class="mb-3 row">
                  <label class="form-label col-md-4">Observaciones:</label>
                  <div class="col-md-8 input-group">
                    <div class="input-group-prepend">
                        <span class="input-group-text">
                          <i class="bi bi-eye"></i>
                        </span>
                    </div>
                    <input class="form-control" type="text" v-model="observ">
                  </div>
                </div>

                <div class="mb-3 row">
                  <label class="form-label col-md-4">Carnet de Identidad:</label>
                  <div class="col-md-8 input-group">
                    <div class="input-group-prepend">
                        <span class="input-group-text">
                          <i class="bi bi-person-vcard"></i>
                        </span>
                    </div>
                    <input class="form-control" type="text" v-model="ci">
                  </div>
                </div>
            
                <div class="mb-3 row">
                  <label class="form-label col-md-4">Lugar de Expedición:</label>
                  <div class="col-md-8 input-group">
                    <div class="input-group-prepend">
                        <span class="input-group-text">
                          <i class="bi bi-list-check"></i>
                        </span>
                    </div>
                    <select v-model="codexp" class="form-control" @change="onChangeCodEstado($event)">
                      <option value='1'>CHUQUISACA</option>
                      <option value='2'>LA PAZ</option>
                      <option value='3'>COCHABAMBA</option>
                      <option value='4'>ORURO</option>
                      <option value='5'>POTOSI</option>
                      <option value='6'>TARIJA</option>
                      <option value='7'>SANTA CRUZ</option>
                      <option value='8'>BENI</option>
                      <option value='9'>PANDO</option>
                      <option value='10'>OTRO</option>
                    </select>   
                  </div>
                </div>

              </div>
              <div class="card-footer">
                <div v-show="errorResponsable" class="form-group row div-error">
                    <div class="text-center text-error">
                        <div v-for="error in errorMostrarMsjResponsable" :key="error" v-text="error">

                        </div>
                    </div>
                </div>
                <div class="mb-3 col-md-3 form-group">
                    <button class="btn btn-primary form-control" type="button" @click="registrarResponsable()"><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-floppy" viewBox="0 0 16 16" >
                      <path d="M11 2H9v3h2z"/>
                      <path d="M1.5 0h11.586a1.5 1.5 0 0 1 1.06.44l1.415 1.414A1.5 1.5 0 0 1 16 2.914V14.5a1.5 1.5 0 0 1-1.5 1.5h-13A1.5 1.5 0 0 1 0 14.5v-13A1.5 1.5 0 0 1 1.5 0M1 1.5v13a.5.5 0 0 0 .5.5H2v-4.5A1.5 1.5 0 0 1 3.5 9h9a1.5 1.5 0 0 1 1.5 1.5V15h.5a.5.5 0 0 0 .5-.5V2.914a.5.5 0 0 0-.146-.353l-1.415-1.415A.5.5 0 0 0 13.086 1H13v4.5A1.5 1.5 0 0 1 11.5 7h-7A1.5 1.5 0 0 1 3 5.5V1H1.5a.5.5 0 0 0-.5.5m3 4a.5.5 0 0 0 .5.5h7a.5.5 0 0 0 .5-.5V1H4zM3 15h10v-4.5a.5.5 0 0 0-.5-.5h-9a.5.5 0 0 0-.5.5z"/>
                    </svg> Guardar</button>
                  </div>
              </div>
            </div>
        </div>
      </div>
      <!----inicio modal-->
      <div class="modal fade" tabindex="-1" :class="{'mostrar' : modal}" role="dialog" aria-labelledby="myModalLabel" style="display: none;" aria-hidden="true">
            <div class="modal-dialog modal-dialog-scrollable modal-dialog-centered modal-lg" role="document">
                <div class="modal-content">
                    <div class="modal-header">
                        <h4 class="modal-title">Buscar Oficinas</h4>
                        <button type="button" class="close btn btn-sm btn-danger" @click="cerrarModal()" aria-label="Close">
                            <span aria-hidden="true">×</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <div class="form-group row">
                            <div class="col-md-6">
                                <div class="input-group mb-3">
                                    <select class="form-control col-md-3" v-model="criterio">
                                    <option value="codofic">Codigo</option>
                                    </select>
                                    <input type="text" v-model="buscar" @keyup.enter="listarOficina(1,buscar,criterio)" class="form-control" placeholder="Ingrese un Codigo">
                                    <button type="submit" @click="listarOficina(1,buscar,criterio)" class="btn btn-primary"><i class="fa fa-search"></i> Buscar</button>
                                </div>
                            </div>
                        </div>
                        <div class="table-responsive">
                            <table class="table table-bordered table-striped table-sm">
                                <thead>
                                    <tr>
                                        <th>Opciones</th>
                                        <th>Código</th>
                                        <th>Oficina</th>
                                        <th>Estado</th>
                                    </tr>
                                </thead>
                                <tbody>
                                    <tr v-for="oficina in arrayOficinas" :key="oficina.id">
                                        <td>
                                            <button type="button" @click="agregarDetalleModal(oficina)" class="btn btn-success btn-sm">
                                            <i class="fa fa-check"></i>
                                            </button>
                                        </td>
                                        <td v-text="oficina.codofic"></td>
                                        <td v-text="oficina.nomofic"></td>
                                        <td>
                                        <div v-if="oficina.api_estado==1">
                                            <span class="me-1 badge badge-pill bg-success">Activo</span>
                                        </div>
                                        <div v-else-if="oficina.api_estado==3">
                                            <span class="me-1 badge badge-pill bg-danger">Inactivo</span>
                                        </div>
                                        
                                    </td>
                                    </tr>                                
                                </tbody>
                            </table>
                            <nav>
                            <ul class="pagination">
                                <li class="page-item" v-if="pagination.current_page > 1">
                                    <a class="page-link" href="#" @click.prevent="cambiarPagina(pagination.current_page - 1,buscar,criterio)">Ant</a>
                                </li>
                                <li class="page-item" v-for="page in pagesNumber" :key="page" :class="[page == isActived ? 'active' : '']">
                                    <a class="page-link" href="#" @click.prevent="cambiarPagina(page,buscar,criterio)" v-text="page"></a>
                                </li>
                                <li class="page-item" v-if="pagination.current_page < pagination.last_page">
                                    <a class="page-link" href="#" @click.prevent="cambiarPagina(pagination.current_page + 1,buscar,criterio)">Sig</a>
                                </li>
                            </ul>
                        </nav>
                        </div>
                    </div>
                    <div class="modal-footer">
                        <button type="button" class="btn btn-secondary" @click="cerrarModal()">Cerrar</button>
                    </div>
                </div>
                <!-- /.modal-content -->
            </div>
            <!-- /.modal-dialog -->
      </div>
    </main>
</template>
<script>
import axios from 'axios';
export default {
  data() {
    return {
        codofic:0,
        codexp:0,
        nomofic:'',
        nomresp:'',
        cargo:'',
        observ:'',
        ci:'',
        arrayOficina:[],
        arrayOficinas:[],
        errorResponsable:0,
        errorMostrarMsjResponsable:[],
        modal : 0,
        criterio:'codofic',
        buscar: '',
        pagination : {
          'total' : 0,
          'current_page' : 0,
          'per_page' : 0,
          'last_page' : 0,
          'from' : 0,
          'to' : 0,
      },
      offset : 3,
    }

  },
  computed:{
            isActived: function(){
                return this.pagination.current_page;
            },
            //Calcula los elementos de la paginación
            pagesNumber: function() {
                if(!this.pagination.to) {
                    return [];
                }
                
                var from = this.pagination.current_page - this.offset; 
                if(from < 1) {
                    from = 1;
                }

                var to = from + (this.offset * 2); 
                if(to >= this.pagination.last_page){
                    to = this.pagination.last_page;
                }  

                var pagesArray = [];
                while(from <= to) {
                    pagesArray.push(from);
                    from++;
                }
                return pagesArray;             

            }
        },
  methods: {
    buscarOficina(){
        let me=this;
        var url= '/oficinas/buscarOficina?filtro=' + me.codofic;
        axios.get(url).then((response)=>{
            me.arrayOficina = response.data.oficina;
            me.codofic=me.arrayOficina.codofic;
            me.nomofic=me.arrayOficina.nomofic;
        })
        .catch(function (error) {
            console.log(error);
            Swal.fire('No se Encontro Oficina','','error')
        });
    },
    listarOficina(page,buscar,criterio){
        let me=this;
        var url= '/oficinas/?page=' + page + '&buscar='+ buscar + '&criterio='+ criterio;
        axios.get(url).then( (response) =>{
            var respuesta= response.data;
            console.log(respuesta);
            me.arrayOficinas = respuesta.oficinas.data;
            me.pagination= respuesta.pagination;
        })
        .catch( (error) =>{
            console.log(error);
        });
    },
    cambiarPagina(page,buscar,criterio){
        let me = this;
        //Actualiza la página actual
        me.pagination.current_page = page;
        //Envia la petición para visualizar la data de esa página
        me.listarOficina(page,buscar,criterio);
    },
    cerrarModal(){
        this.modal=0;
        this.arrayOficina=[];
    }, 
    abrirModal(){  
        this.modal = 1;
        this.listarOficina(1,this.buscar,this.criterio);
    },
    agregarDetalleModal(data){
      this.codofic=data.codofic;
      this.nomofic=data.nomofic;
      this.cerrarModal();
    },
    onChangeCodEstado(event) {
        this.codexp= (event.target.value);
    },
    registrarResponsable(){
      if (this.validarResponsable()){ return;}
      axios.post('/responsable/registrar',{
            'codofic': this.codofic,
            'nomresp': this.nomresp,
            'cargo' : this.cargo,
            'ci' : this.ci,
            'codexp' : this.codexp,
            'observ' : this.observ,
        }).then((response)=>{
          Swal.fire(response.data.message, "", "success");
          this.codofic=0;
          this.nomresp='';
          this.cargo='';
          this.ci='';
          this.codexp=0;
          this.observ='';
        }).catch((error)=>{
            console.log(error);
            Swal.fire(response.data.message, "", "error");
        });
    },
    validarResponsable(){
        this.errorResponsable=0;
        this.errorMostrarMsjResponsable =[];

        if (!this.nomresp) {this.errorMostrarMsjResponsable.push("El nombre del Responsable no puede estar vacío.");};
        if (!this.cargo) {this.errorMostrarMsjResponsable.push("Cargo del Responsable no puede estar vacío.");};
        if (!this.ci){ this.errorMostrarMsjResponsable.push("El Nro. de Carnet no puede estar vacío.");};
        if (this.codofic==0){ this.errorMostrarMsjResponsable.push("Seleccione una Oficina.");};
        if (this.codexp==0){ this.errorMostrarMsjResponsable.push("Seleccione un Departamento.");};
        if (this.errorMostrarMsjResponsable.length) this.errorResponsable = 1;

        return this.errorResponsable;
    },
  },
  mounted() {
   
  }
}
</script>
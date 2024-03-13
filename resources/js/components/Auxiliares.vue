<template>
   <main class="app-content">
        <div class="app-title">
            <h1><i class="bi bi-calculator"></i> Auxiliares</h1>
            <br>
            <button type="submit" @click="revisarNuevos()" class="btn btn-primary"><i class="bi bi-arrow-clockwise"></i> Actualizar Datos</button>
        </div>
        <div class="form-group row">
        <div class="col-md-6">
            <div class="input-group">
                <input type="text" v-model="buscar" @keyup.enter="listarAuxiliar(1,buscar,'nomaux')" class="form-control" placeholder="Texto a buscar">
                <button type="submit" @click="listarAuxiliar(1,buscar,'nomaux')" class="btn btn-primary"><i class="fa fa-search"></i> Buscar</button>
            </div>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
          <div class="tile">
            <div class="tile-body table-responsive">
                 <table class="table table-bordered table-striped table-sm">
                    <thead>
                        <tr>
                            <th>Opciones</th>
                            <th>Nombre</th>
                            <th>Oficina</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="auxiliar in arrayAuxiliar" :key="auxiliar.id">
                            <td>
                                <button type="button" @click="abrirModal(auxiliar)" class="btn btn-warning btn-sm" data-bs-toggle="tooltip" data-bs-placement="left" data-bs-title="Editar">
                                  <i class="bi bi-pencil-square"></i>
                                </button>
                            </td>
                            <td v-text="auxiliar.nomaux"></td>
                            <td v-text="auxiliar.nombre"></td>
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
        </div>
    </div> <!-- Fin ejemplo de tabla Listado -->
            <!--Inicio del modal agregar/actualizar-->
            <div class="modal fade" tabindex="-1" :class="{'mostrar' : modal}" role="dialog" aria-labelledby="myModalLabel" style="display: none;" aria-hidden="true">
                <div class="modal-dialog modal-dialog-scrollable modal-dialog-centered modal-lg" role="document">
                    <div class="modal-content">
                        <div class="modal-header">
                            <h4 class="modal-title"> Actualizar Auxiliar</h4>
                            <button type="button" class="close btn btn-danger btn-sm" @click="cerrarModal()" aria-label="Close">
                              <span aria-hidden="true">×</span>
                            </button>
                        </div>
                        <div class="modal-body">
                            <form action="" method="post" enctype="multipart/form-data" class="form-horizontal">
                                <div class="form-group row mb-3">
                                    <label class="col-md-3" for="text-input">Nombre Auxiliar</label>
                                    <div class="col-md-9">
                                        <input type="text" v-model="nomaux" class="form-control">                                        
                                    </div>
                                </div>
                                <div class="form-group row mb-3">
                                    <label class="col-md-3" for="text-input">Grupo Contable</label>
                                    <div class="col-md-9">
                                      <select class="form-control" v-model="codcont" @change="onChangeCodigo($event)">
                                        <option v-for="gcont in arrayContables" :key="gcont.id" :value="gcont.codcont" v-text="gcont.nombre"></option>
                                    </select>                            
                                    </div>
                                </div>
                                <div v-show="errorAuxiliar" class="form-group row div-error">
                                    <div class="text-center text-error">
                                        <div v-for="error in errorMostrarMsjAuxiliar" :key="error" v-text="error">

                                        </div>
                                    </div>
                                </div>

                            </form>
                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" @click="cerrarModal()">Cerrar</button>
                            <button type="button" class="btn btn-primary" @click="actualizarAuxiliar()">Actualizar</button>
                        </div>
                    </div>
                    <!-- /.modal-content -->
                </div>
                <!-- /.modal-dialog -->
            </div>
            <!--Fin del modal-->
    </main>
</template>
<script>
import axios from 'axios';
export default {
  data() {
    return {
      id:0,
      nomaux:'',
      codcont:1,
      arrayAuxiliar : [],
      arrayContables : [],
      modal : 0,
      errorAuxiliar : 0,
      errorMostrarMsjAuxiliar : [],
      pagination : {
          'total' : 0,
          'current_page' : 0,
          'per_page' : 0,
          'last_page' : 0,
          'from' : 0,
          'to' : 0,
      },
      offset : 3,
      criterio : 'nomaux',
      buscar : ''
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
    listarAuxiliar (page,buscar,criterio){
        let me=this;
        var url= '/auxiliar?page=' + page + '&buscar='+ buscar + '&criterio='+ criterio;
        axios.get(url).then( (response) =>{
            var respuesta= response.data;
            me.arrayAuxiliar = respuesta.auxiliares.data;
            me.pagination= respuesta.pagination;
        })
        .catch( (error)=> {
            console.log(error);
        });
    },
    listarContables (){
        let me=this;
        var url= '/contable';
        axios.get(url).then( (response) =>{
            var respuesta= response.data;
            me.arrayContables = respuesta.codigos;
        })
        .catch( (error)=> {
            console.log(error);
        });
    },
    onChangeCodigo(event) {
        this.codcont=(event.target.value);
    },
    cambiarPagina(page,buscar,criterio){
        let me = this;
        me.pagination.current_page = page;
        me.listarAuxiliar(page,buscar,criterio);
    },
    abrirModal(data = []){
      
      this.modal=1;
      this.id = data['id'];
      this.nomaux=data['nomaux'];
      this.codcont=data['codcont'];
      this.codaux=data['codaux'];
    },
    cerrarModal(){
      this.modal=0;
      this.nomaux='';
    },
    actualizarAuxiliar(){
          let me = this;
          axios.put('/auxiliar/update',{
            'id':this.id,
            'nomaux':this.nomaux,
            'codcont':this.codcont,
            'codaux':this.codaux,
          }).then((response)=>{
              me.cerrarModal();
              Swal.fire(response.data.message,"","info");
              me.listarAuxiliar(1,'','nomaux');
          }).catch((error)=>{
            Swal.error('Hubo un Error');
              console.log(error);
          }); 
      },  
  },
  mounted() {
    this.listarAuxiliar(1,this.buscar,this.criterio);
    this.listarContables();
  }
}
</script>
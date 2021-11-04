<template>
    <div>
        <h1 class="text-center">Gestionar articulos</h1>
        <hr />
        <!-- Button trigger modal -->
        <button
            @click="
                modificar = false;
                abrirModal();
            "
            type="button"
            class="btn btn-primary my-4"
        >
            Nuevo articulo
        </button>

        <!-- Modal -->
        <div class="modal" :class="{ mostrar: modal }">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h5 class="modal-title" id="exampleModalLabel">
                            {{ tituloModal }}
                        </h5>
                        <button
                            @click="cerrarModal()"
                            type="button"
                            class="close"
                            data-dismiss="modal"
                            aria-label="Close"
                        >
                            <span aria-hidden="true">&times;</span>
                        </button>
                    </div>
                    <div class="modal-body">
                        <div>
                            <label for="nombre">Nombre</label>
                            <input v-model="articulo.nombre" type="text" class="form-control"  id="nombre" placeholder="Nombre de articulo">                            
                        </div>                        
                        <div>
                            <label for="descripcion">Descripcion</label>
                            <input v-model="articulo.descripcion" type="text" class="form-control"  id="descripcion" placeholder="Descripcion de articulo">                            
                        </div>                        
                        <div>
                            <label for="stock">Stock</label>
                            <input v-model="articulo.stock" type="number" class="form-control"  id="stock" placeholder="Stock de articulo">                            
                        </div>                        
                    </div>
                    <div class="modal-footer">
                        <button
                            @click="cerrarModal()"
                            type="button"
                            class="btn btn-secondary"
                            data-dismiss="modal"
                        >
                            Cerrar
                        </button>
                        <button @click="guardar()" type="button" class="btn btn-success">
                            Guardar
                        </button>
                    </div>
                </div>
            </div>
        </div>

        <table class="table table-striped">
            <thead class="thead-dark">
                <tr>
                    <th scope="col">#</th>
                    <th scope="col">Nombre</th>
                    <th scope="col">Descripcion</th>
                    <th scope="col">Stock</th>
                    <th scope="col" colspan="2" class="text-center">Accion</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="art in articulos" :key="art.id">
                    <th scope="row">{{ art.id }}</th>
                    <td>{{ art.nombre }}</td>
                    <td>{{ art.descripcion }}</td>
                    <td>{{ art.stock }}</td>
                    <td>
                        <button
                            @click="
                                modificar = true;
                                abrirModal(art);
                            "
                            type="button"
                            class="btn btn-secondary"
                        >
                            Modificar articulo
                        </button>
                    </td>
                    <td>
                        <button
                            @click="eliminar(art.id)"
                            class="btn btn-danger"
                        >
                            Eliminar
                        </button>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
export default {
    data() {
        return {
            articulo:{
                nombre:'',
                descripcion:'',
                stock:1                
            },
            id:0,
            modificar: true,
            modal: 0,
            tituloModal: "",
            articulos: [],
        };
    },
    methods: {
        async listar() {
            const res = await axios.get("/articulos");
            this.articulos = res.data;
        },
        async eliminar(id) {
            const res = await axios.delete("/articulos/" + id);
            this.listar();
        },
        async guardar() {
            if (this.modificar) {
                const res = await axios.put("/articulos/"+this.id, this.articulo);
            }else{
                const res = await axios.post("/articulos",this.articulo);
            }
            this.cerrarModal();
            this.listar();
            
            
        },
        abrirModal(data={}) {
            this.modal = 1;
            if (this.modificar) {
                this.id=data.id;
                this.tituloModal = "Modificar articulo";
                this.articulo.nombre = data.nombre;
                this.articulo.descripcion = data.descripcion;
                this.articulo.stock = data.stock;
            } else {
                this.id=0;
                this.tituloModal = "Nuevo articulo";
                this.articulo.nombre = '';
                this.articulo.descripcion = '';
                this.articulo.stock = 1;
            }
        },
        cerrarModal() {
            this.modal = 0;
        },
    },
    created() {
        this.listar();
    },
};
</script>
<style>
.mostrar {
    display: list-item;
    opacity: 1;
    background: rgb(44, 38, 75, 0.849);
}
</style>

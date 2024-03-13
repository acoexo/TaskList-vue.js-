<template>
    <div>
        <h1 class="display-4 text-center">Listado de Tareas</h1>
        <hr>
        <div class="row">
            <div class="col-lg-8 offset-lg-2">
                <div class="card mt-4">
                    <div class="card-body">
                        <div class="input-group">
                            <input type="text" v-model="tarea" class="form-control form-control-lg"
                                placeholder="Agregar Tarea" @input="validarInput">
                            <div class="input-group-append">
                                <button v-on:click="agregarTarea()" class="btn btn-success btn-lg">Agregar</button>
                            </div>
                        </div>
                        <br>
                        <h6 id="error" class="d-none mb-4"></h6>
                        <h5 v-if="listTareas.length == 0">No hay tareas pendientes</h5>
                        <ul class="list-group">
                            <li v-for="(tarea, index) of listTareas" :key="index"
                                class="list-group-item d-flex justify-content-between">
                                <span class="cursor" v-bind:class="{ 'text-success': tarea.estado }"
                                    v-on:click="editarTarea(tarea, index)">
                                    <i
                                        v-bind:class="[tarea.estado ? 'fa-regular fa-circle-check' : 'fa-regular fa-circle']"></i>

                                </span>
                                {{ tarea.nombre }}
                                <span class="text-danger cursor" v-on:click="eliminarTarea(index)">
                                    <i class="fa-solid fa-trash-can"></i>
                                </span>
                            </li>
                        </ul>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TaskList',
    data() {
        return {
            tarea: '',
            listTareas: []
        }
    },
    methods: {
        existeTarea(tarea) {
            return this.listTareas.some(item => tarea === item.nombre);
        },
        validarTarea(tarea) {
            let regex = /^[a-zA-Z0-9,.;:/%$€@\s]+$/;
            return regex.test(tarea);
        },
        agregarTarea() {
            const tarea = {
                nombre: this.tarea,
                estado: false
            }
            if (!this.existeTarea(this.tarea)) {
                if (this.tarea === '') {
                    this.mostrarMensajeError("No puedes introducir una tarea vacía");
                } else {
                    if (this.validarTarea(this.tarea)) {
                        this.listTareas.push(tarea);
                    }
                }
            } else {
                this.mostrarMensajeError("Ya existe la tarea " + this.tarea);
            }

            this.tarea = '';
        },
        validarInput() {
            if (this.tarea !== '' && !this.validarTarea(this.tarea)) {
                this.mostrarMensajeError("Carácter no válido");
                this.tarea = this.tarea.replace(/[^a-zA-Z0-9,.;:/%$€@]/g, '');
            }
        },
        eliminarTarea(index) {
            this.listTareas.splice(index, 1);
        },
        editarTarea(tarea, index) {
            this.listTareas[index].estado = !this.listTareas[index].estado;
        },
        ocultarMensajeError() {
            let error = document.getElementById('error');
            error.innerHTML = "";
            error.classList = 'd-none mb-4';
        },
        mostrarMensajeError(mensaje) {
            let error = document.getElementById('error');
            error.innerHTML = mensaje;
            error.classList = 'd-flex mb-4';
            setTimeout(this.ocultarMensajeError, 2000);
        }
    }
}
</script>

<style scoped>
.cursor {
    cursor: pointer;
}
</style>
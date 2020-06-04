<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-10">
                <div class="card">
                    <div class="card-header">
                        <Strong>Usuarios</Strong> 
                        <!-- <button class="float-right btn btn-primary btn-sm">Crear Usuario</button> -->
                        <b-button v-b-modal.AddModal class="float-right btn btn-primary btn-sm">Crear Usuario</b-button>
                    </div>

                    <div class="card-body">
                        <table class="table table-striped">
                            <thead>
                                <tr>
                                    <th scope="col">Nombre Completo</th>
                                    <th scope="col">Correo</th>
                                    <th scope="col">Fecha de Nacimiento</th>
                                    <th scope="col">Celular</th>
                                    <th scope="col">Genero</th>
                                    <th scope="col">Opciones</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(user, index) in users" :key="index">
                                    <th scope="row">{{ user.name}}</th>
                                    <td>{{ user.email }}</td>
                                    <td>{{ user.birthdate }}</td>
                                    <td>{{ user.phone }}</td>
                                    <td v-if="user.gender == 'M'" >Masculino</td>
                                    <td v-if="user.gender == 'F'" >Femenino</td>
                                    <td>
                                        <button class="btn btn-warning" @click="editUser(user)" > Editar </button>
                                        <button class="btn btn-danger" @click="deleteUser(user)"> Eliminar </button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <!-- ADD USERS -->
        <b-modal id="AddModal" title="Agregar Usuario" hide-footer>
            <form @submit.prevent="AgregarUser">
                <div class="form-group">
                    <label for="name">Nombre Completo</label>
                    <input type="name" v-model="addUser.name" class="form-control" id="name" required>
                    <!-- <small id="emailHelp" class="form-text text-muted"></small> -->
                </div>

                <div class="form-group">
                    <label for="email">Correo</label>
                    <input type="email" v-model="addUser.email" class="form-control" id="email" required>
                    <!-- <small id="emailHelp" class="form-text text-muted"></small> -->
                </div>

                <div class="form-group">
                    <label for="exampleInputPassword1">Password</label>
                    <input type="password" v-model="addUser.password" class="form-control" id="exampleInputPassword1" required minlength='8'>
                </div>

                <div class="form-group">
                    <label for="date">Fecha de Nacimiento</label>
                    <input type="date" v-model="addUser.birthdate" class="form-control" id="date" required>
                    <!-- <small id="emailHelp" class="form-text text-muted"></small> -->
                </div>
                
                <div class="form-group">
                    <label for="phone">Numero de Celular</label>
                    <input type="number" v-model="addUser.phone" class="form-control" id="phone" required maxlength="10">
                    <!-- <small id="emailHelp" class="form-text text-muted"></small> -->
                </div>

                <div class="form-group">
                    <label for="gender">Sexo</label>
                    <select v-model="addUser.gender" class="form-control" id="gender" required>
                        <option value="M" selected>Masculino</option>
                        <option value="F">Femenino</option>
                    </select>
                    <!-- <small id="emailHelp" class="form-text text-muted"></small> -->
                </div>
            
                <button type="submit" class="btn btn-primary">Submit</button>
                <button type="button" v-on:click="hideCategoryModal('AddModal')" class="btn btn-default">Cancelar</button>
            </form>
        </b-modal>

    </div>
</template>

<script>
    export default {
        data() {
            return {
                users : [],
                addUser: {
                    name : '',
                    email : '',
                    password : '',
                    birthdate : '',
                    phone : '',
                    gender : '',
                },
                errors : [],
            }
        },
        mounted() {
            // console.log('Component mounted.')
            this.readUser();
        },
        methods: {
            readUser(){
                axios.get('http://localhost:8000/users')
                .then((response) => {
                    // console.log(response.data);
                    this.users = response.data;
                    // console.log(this.users);
                     
                })
                .catch((error) => {
                    console.log(error);
                });
            },
            hideModal(idModal){
                this.$bvModal.hide(idModal);
            },
            AgregarUser(){

                // console.log(this.addUser);
                
                axios.post('/users', {
                    name: this.addUser.name,
                    email: this.addUser.email,
                    password: this.addUser.password,
                    birthdate: this.addUser.birthdate,
                    phone: this.addUser.phone,
                    gender: this.addUser.gender,
                })
                .then(function (response) {
                    // console.log(response);
                    alert('Usuario Creado');
                    this.readUser();
                    this.hideModal('AddModal');
                })
                .catch(function (error) {
                    console.log(error);
                });

                // this.hideModal('AddModal');
                // this.readUser();
                
            },
            hideModal(idModal){
                this.$bvModal.hide(idModal);
            },
            editUser(user){

            },
            deleteUser(user){
                if(!window.confirm(`Estas seguro de eliminar al usuario ${user.name}`)){
                    return;
                }

                var url = '/users/' + user.id;
                axios.delete(url)
                .then(function (response) {
                    // console.log(response);
                    alert('Usuario Eliminado');
                    // this.readUser();
                    // this.hideModal('AddModal');
                })
                .catch(function (error) {
                    console.log(error);
                }); 

                this.readUser();
            }
        }
    }
</script>

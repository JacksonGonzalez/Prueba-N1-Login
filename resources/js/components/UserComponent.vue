<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-10">
                <div class="card">
                    <div class="card-header">
                        <Strong>Usuarios</Strong> 
                        <!-- <button class="float-right btn btn-primary btn-sm">Crear Usuario</button> -->
                        <b-button v-b-modal.AddUser class="float-right btn btn-primary btn-sm">Crear Usuario</b-button>
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
                                </tr>
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
        </div>

        <!-- ADD USERS -->
        <b-modal id="AddUser" title="BootstrapVue">
            <p class="my-4">Hello from modal!</p>
        </b-modal>

    </div>
</template>

<script>
    export default {
        data() {
            return {
                users : [],
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
            }
        }
    }
</script>

<template>
    <div class="about">
        <h5 class="mb-5 mt-5">Agenda de contactos</h5>

        <div>

            <div class="mb-3">
                <div class="input-group">
                    <span class="input-group-text" id="name-search-text"><i class="bi bi-search"></i> Buscar por nombre </span>
                    <input type="search" class="form-control" id="name-search"
                           @search="this.toSearch = $event.target.value">
                </div>
            </div>
        </div>

        <div class="card">
            <div class="card-body">
                <form @submit.prevent="save()">
                    <table class="table table-striped">
                        <thead>
                        <tr><th>
                                -
                            </th>
                            <th>
                                -
                            </th>
                            
                            <th>
                                <input type="text" placeholder="name" class="form-control" id="name" v-model="newLink.name">
                            </th>
                            <th>
                                <input type="email" placeholder="email" class="form-control" id="email" v-model="newLink.email">
                            </th>
                            <th>
                                <input type="text" placeholder="address" class="form-control" id="address" v-model="newLink.address">
                            </th>
                            <th>
                                <input type="number" placeholder="phone" class="form-control" id="phone" v-model="newLink.phone">
                            </th>
                            <th>
                                <input type="text" placeholder="country" class="form-control" id="country" v-model="newLink.country">
                            </th>
                            <th>
                                <input type="text" placeholder="city" class="form-control" id="city" v-model="newLink.city">
                            </th>
                            <th>
                                <button type="submit" class="btn btn-primary">Agregar</button>
                            </th>
                        </tr>
                        <tr>
                            <th scope="col">#</th>
                            <th scope="col">Id</th>
                            <th scope="col">Name</th>
                            <th scope="col">Email</th>
                            <th scope="col">Address</th>
                            <th scope="col">Phone</th>
                            <th scope="col">Country</th>
                            <th scope="col">City</th>
                            <th></th>
                        </tr>
                        </thead>

                        <tbody>
                        <tr v-for="(link, index) in getList()" :key="index">
                            <th scope="row">{{1+index}}</th>
                            <td>{{link.id}}</td>
                            <td>{{link.name}}</td>
                            <td>{{link.email}}</td>
                            <td>{{link.address}}</td>
                            <td>{{link.phone}}</td>
                            <td>{{link.country}}</td>
                            <td>{{link.city}}</td>
                            <td>
                                <button type="button" class="btn btn-info btn-sm" @click="edit(index)"><i
                                        class="bi bi-pen"></i></button>

                                <button type="button" class="btn btn-danger btn-sm" @click="remove(index)"><i
                                        class="bi bi-trash3"></i></button>
                            </td>
                        </tr>
                        </tbody>
                    </table>
                </form>
            </div>
        </div>

        <!-- Modal -->
        <div class="modal fade" id="editModal" tabindex="-1" aria-labelledby="editModalLabel" aria-hidden="true">
            <div class="modal-dialog">
                <div class="modal-content">
                    <div class="modal-header">
                        <h1 class="modal-title fs-5" id="editModalLabel">Editar</h1>
                        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
                    </div>
                    <form @submit.prevent="saveEdit()">
                        <div class="modal-body" v-if="itemSelected">
                            <div class="mb-3">
                                <label for="updateName" class="form-label">Name</label>
                                <input type="text" v-model="itemSelected.name" class="form-control"
                                       id="updateName">
                            </div>
                            <div class="mb-3">
                                <label for="updateEmail" class="form-label">Email</label>
                                <input type="text" v-model="itemSelected.email" class="form-control" id="updateEmail">
                            </div>
                            <div class="mb-3">
                                <label for="updateAddress" class="form-label">Address</label>
                                <input type="text" v-model="itemSelected.address" class="form-control"
                                       id="updateAddress">
                            </div>
                            <div class="mb-3">
                                <label for="updatePhone" class="form-label">Phone</label>
                                <input type="text" v-model="itemSelected.phone" class="form-control" id="updatePhone">
                            </div>
                            <div class="mb-3">
                                <label for="updateCountry" class="form-label">Country</label>
                                <input type="text" v-model="itemSelected.country" class="form-control"
                                       id="updateCountry">
                            </div>
                            <div class="mb-3">
                                <label for="updatecity" class="form-label">city</label>
                                <input type="text" v-model="itemSelected.city" class="form-control" id="updatecity">
                            </div>

                        </div>
                        <div class="modal-footer">
                            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cancelar</button>
                            <button type="submit" class="btn btn-primary">Guardar cambios</button>
                        </div>
                    </form>
                </div>
            </div>
        </div>

    </div>
</template>

<script>
    export default {
        data() {
            return {
                newLink: {
                    id: '',
                    name: '',
                    email: '',
                    address: '',
                    phone: '',
                    country: '',
                    city: ''
                },
                linksArray: [
                {
                    id: 1,
                    name: "Alice Johnson",
                    email: "alice.johnson@example.com",
                    address: "123 Maple Street",
                    phone: "123-456-7890",
                    country: "USA",
                    city: "New York"
                    },
                    {
                    id: 2,
                    name: "Bob Smith",
                    email: "bob.smith@example.com",
                    address: "456 Oak Avenue",
                    phone: "987-654-3210",
                    country: "Canada",
                    city: "Toronto"
                    },
                    {
                    id: 3,
                    name: "Carol White",
                    email: "carol.white@example.com",
                    address: "789 Pine Road",
                    phone: "555-123-4567",
                    country: "UK",
                    city: "London"
                    },
                    {
                    id: 4,
                    name: "David Brown",
                    email: "david.brown@example.com",
                    address: "321 Elm Street",
                    phone: "444-555-6666",
                    country: "Australia",
                    city: "Sydney"
                    },
                    {
                    id: 5,
                    name: "Emily Davis",
                    email: "emily.davis@example.com",
                    address: "654 Spruce Lane",
                    phone: "333-444-5555",
                    country: "USA",
                    city: "Los Angeles"
                }

                ],
                itemSelected: null,
                indexSelected: null,
                toSearch: ''
            }
        },
        methods: {
            save() {
                let id = this.linksArray.length
                console.log(id)
                // Validar que los campos no estén vacíos
                if (this.newLink.name && this.newLink.email && this.newLink.address && 
                this.newLink.phone && this.newLink.country && this.newLink.city) {
                    // Agregar un nuevo objeto al array
                    /**
                     * const newObj {type: this.newlink.type}
                     * 
                     * const newObj = {...this.newLink}
                     * */
                     this.newLink.id= id+1;
                    this.linksArray.push({...this.newLink});
                    // Limpiar los campos del formulario
                    
                    this.newLink.id= '';
                    this.newLink.name= '';
                    this.newLink.email= '',
                    this.newLink.address= '';
                    this.newLink.phone= '';
                    this.newLink.country= '';
                    this.newLink.city= '';

                } else {
                    alert("Por favor, completa todos los campos.");
                }
            },
            remove(index) {
                if (confirm("¿Está seguro de eliminar este ítem?")) {
                    this.linksArray.splice(index, 1);
                }
            },
            edit(index) {
                this.itemSelected = {...this.linksArray[index]};
                this.indexSelected = index;
                const editModal = new bootstrap.Modal(document.getElementById('editModal'));
                editModal.show();
            },
            saveEdit() {
                this.linksArray[this.indexSelected] = {...this.itemSelected};

                const modalElement = document.getElementById('editModal');
                const modalInstance = bootstrap.Modal.getInstance(modalElement) || new bootstrap.Modal(modalElement);
                modalInstance.hide();

                this.itemSelected = null;
                this.indexSelected = null;
            },
            getList() {
                let result = this.linksArray.filter((item) => {
                    if (this.toSearch) {
                        return item.name.includes(this.toSearch);
                    }
                    return true;
                });
                return result.filter((item) => {
                    if (this.toFilter) {
                        return item.type === this.toFilter;
                    }
                    return true;
                });
                // return this.linksArray;
            }

        }
    }
</script>

<style>
    .btn {
        margin-right: 3px;
    }
    .card{
        overflow-x: auto;
    }
</style>
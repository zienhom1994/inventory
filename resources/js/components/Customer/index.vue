<template>
    <div>
        <div class="row">
            <router-link to="/store-customer" class="btn btn-primary">Add Customer</router-link>

        </div>
        <br>
        <input class="form-control" v-model="searchTerm" style="width: 300px;" type="text" placeholder="Search Here">

        <div class="container-fluid" id="container-wrapper">
            <div class="d-sm-flex align-items-center justify-content-between mb-4">

            </div>

            <div class="row">
                <div class="col-lg-12 mb-4">
                    <!-- Simple Tables -->
                    <div class="card">
                        <div class="card-header py-3 d-flex flex-row align-items-center justify-content-between">
                            <h6 class="m-0 font-weight-bold text-primary">Customers</h6>
                        </div>
                        <div class="table-responsive">
                            <table class="table align-items-center table-flush">
                                <thead class="thead-light">
                                <tr>
                                    <th>Name</th>
                                    <th>Email</th>
                                    <th>Address</th>
                                    <th>Phone No.</th>

                                    <th>Photo</th>
                                    <th>Action</th>

                                </tr>
                                </thead>
                                <tbody>
                                <tr v-for="customer in filterSearch" :key="customer.id">
                                    <td>{{customer.name}}</td>
                                    <td>{{customer.email}}</td>
                                    <td>{{customer.address}}</td>
                                    <td>{{customer.phone}}</td>



                                    <td><img :src="customer.photo" id="customer_photo"></td>
                                    <!--                                    <td><span class="badge badge-success">Delivered</span></td>-->
                                    <td>
                                        <a @click="deleteCustomer(customer.id)" class="btn btn-sm btn-danger">Delete</a>
                                        <router-link :to="{name:'edit-customer', params:{id:customer.id}}"
                                                     class="btn btn-sm btn-info">Edit
                                        </router-link>

                                    </td>
                                </tr>

                                </tbody>
                            </table>
                        </div>
                        <div class="card-footer"></div>
                    </div>
                </div>
            </div>
            <!--Row-->
        </div>


    </div>
</template>


<script type="text/javascript">

    export default {

        created() {
            if (!User.loggedIn()) {
                this.$router.push('home')
            }

            this.allCustomers();

        },
        data() {
            return {
                customers:[],
                searchTerm: ''

            }

        },
        computed: {
            filterSearch() {
                return this.customers.filter(customer => {
                    return customer.name.match(this.searchTerm)

                })
            }
        },
        methods: {
            allCustomers() {
                axios.get('/api/customer/')
                    .then(({data}) => (this.customers = data))
                    .catch()

            },
            deleteCustomer(id) {
                Swal.fire({
                    title: 'Are you sure?',
                    text: "You won't be able to revert this!",
                    icon: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Yes, delete it!'
                }).then((result) => {
                    if (result.isConfirmed) {
                        axios.delete('/api/customer/' + id)
                            .then(() => {
                                this.customers = this.customers.filter(customer => {
                                    return customer.id != id;

                                })

                            })
                            .catch(() => {
                                this.$router.push({name: 'customer'})
                            })
                        Swal.fire(
                            'Deleted!',
                            ' Customer deleted.',
                            'success'
                        )
                    }
                })


            }


        },


    }


</script>


<style type="text/css">
    #customer_photo {
        height: 40px;
        width: 40px;
    }

    a.btn.btn-sm {
        color: white;
    }


</style>

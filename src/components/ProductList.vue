<template>
    <div class="container">
        <div class="row">
            <div class="col-sm-5 bg-light well-lg">
                <h1>Agregar producto</h1>
                <hr>
                <div class="text-left">
                    <label for="">SKU <span class="text-danger" v-if="sku==''">(*)</span></label>
                    <input type="text" v-model="sku" class="form-control" placeholder="SKU">
                    <br>
                    <label for="">Nombre <span class="text-danger" v-if="name==''">(*)</span></label>
                    <input type="text" v-model="name" class="form-control" placeholder="Nombre">
                    <br>
                    <label for="">Cantidad <span class="text-danger" v-if="quantity==''">(*)</span></label>
                    <input type="text" v-model="quantity" class="form-control" placeholder="Cantidad">
                    <br>
                    <label for="">Precio <span class="text-danger" v-if="price==''">(*)</span></label>
                    <input type="number" v-model="price" class="form-control" placeholder="$0.00" step="0.01">
                    <hr>
                    <div class="text-center">
                        <button @click="saveData()" class="btn btn-sm btn-primary">Agregar</button>
                    </div>
                </div>
            </div>
            <div class="col-sm-7 bg-light">
                <h1>Litado de productos</h1>
                <table class="table table-sm table-striped table-bordered">
                    <thead>
                        <tr><th colspan="4"><h4 class="text-center">Carrito de compras</h4></th></tr>
                        <tr>
                            <th>Numero de orden</th>
                            <th>SKU</th>
                            <th>Nombre</th>
                            <th>Cantidad</th>
                            <th>Precio</th>
                        </tr>
                    </thead>
                    <tbody>
                        <tr v-for="data in data.orders" :key="data.id">
                            <td v-text="data.number"></td>
                            <td v-text="data.items[0].sku"></td>
                            <td v-text="data.items[0].name"></td>
                            <td v-text="data.items[0].quantity"></td>
                            <td v-text="'$'+data.items[0].price"></td>
                        </tr>
                    </tbody>
                </table>  
                <hr>
                <div class="text-right">
                    <button @click="checkOut()" class="btn btn-sm btn-primary">Pagar</button>
                </div>
                <br>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios';
    import Swal from 'sweetalert2'

    export default {
        data(){
            return{
                config:{
                    headers:{
                        Authorization: "Bearer eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJwUGFINU55VXRxTUkzMDZtajdZVHdHV3JIZE81cWxmaCIsImlhdCI6MTYwNTY0NDA0NzA1OH0.skfIY_7CAANkxmhoq37OI4jYRE8flx1ENq1v1VaRevJiroYNFQYz7Oy6hL1YZ1OJkevXSQFuLMHTqY0w6d5nPQ"
                    }
                },
                data:[],
                sku:'',
                name:'',
                quantity:'',
                price:'',
            }
        },
        methods:{
            getCartData(){
                axios.get("https://eshop-deve.herokuapp.com/api/v2/orders", this.config).then(
                    response => this.data = response.data
                ).catch(error=>console.log(error));
            },
            saveData(){
                
                if(this.sku=='' || this.name=='' || this.quantity=='' || this.price<=0){
                    Swal.fire({
                        icon: 'error',
                        title: 'Oops...',
                        text: 'Debes completar todos los campos!',
                    });
                    return false;
                }else{
                    Swal.fire({
                        position: 'center',
                        icon: 'success',
                        title: 'El producto se ha agregado correctamente',
                        showConfirmButton: false,
                        timer: 1500
                    });

                    this.sku='';
                    this.name='';
                    this.quantity='';
                    this.price='';
                }
                
            },
            checkOut(){
                Swal.fire({
                    title: '¿Estas seguro?',
                    text: "Estas apunto de comprar tus artículos!",
                    icon: 'warning',
                    showCancelButton: true,
                    confirmButtonColor: '#3085d6',
                    cancelButtonColor: '#d33',
                    confirmButtonText: 'Si, comprar!',
                    cancelButtonText: 'Cancelar'
                }).then((result) => {
                    if (result.isConfirmed) {
                        Swal.fire(
                        'Felicidades!',
                        'Tu compra se ha realizado.',
                        'success'
                        )
                    }
                });
            }
        },
        mounted(){
            this.getCartData();
        }
    }
</script>

<style>

</style>
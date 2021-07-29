<template>
    <div>
        <div class="container">

            <div class="grid">

                <div class="caja1">
                    <div v-for="(item,i) in posts" :key="i"  @click="getDetail(item)">
                        
                        <div class="lista-de-pajaros">
                            <div>
                                <img class="caja-imagen" :src="item.images.main">
                            </div>
                            
                            <div class="nombre-pajaro">
                               <h5> {{item.name.spanish}}</h5>
                            </div>
                        </div>    
                        
                        <hr>
                    </div>
                </div>

                <div class="caja2">

                    <div v-if="pajaroArray"> 

                    
                        <div v-for="(pajaro,i) in pajaroArray" :key="i">

                            <div class="titulo-detalle">

                                    <h1 class="nombre-pajaro"> {{pajaro.name.spanish}}</h1>

                                    <h4 class="nombre-latin">
                                        <i>({{pajaro.name.latin}})</i>
                                    </h4>
                            <br/>
                            </div>

                            <div class="pajaro-detail">

                                <img class="caja-imagen-detalle" :src="pajaro.images.main">

                                <h5 class="detalle">{{detalleArray.habitat}}</h5>

                            </div>

                            <hr/>
                    
                            
                            <div class="mapa">
                                <br/>
                                <h5><strong>{{detalleArray.map.title}}</strong></h5>
                                <img class="imagen" :src="detalleArray.map.image">
                            </div>
                            

                        
                        </div>
                    </div>

                    <div v-else>
                        Seleccione un Pajaro
                    </div>


                </div>

            </div>
                
        </div>

    </div>
</template>

<script>

import axios from 'axios'

export default {
    name: 'Apirest',
    
    data(){
        
        return{
            posts:[],
            pajaroArray:[]=[],
            detalleArray:[]=[]
           
        }
    },
    methods:{
        getDetail(row){      
      
            let thisLocal = this;      
            this.pajaroArray.pop();
            console.log(typeof(this.pajaroArray));
            this.pajaroArray.push(row);
            console.log(row._links.self);

            axios.get(row._links.self)
            .then( function( response ) {
            //console.log(response.data)
            thisLocal.detalleArray = response.data;
            console.log('detallepajaro',thisLocal.detalleArray);
            console.log(thisLocal.detalleArray.habitat);    
        })

            console.log("arreglo pajaro",this.pajaroArray);
            //console.log(row);
            //console.log(typeof(thisLocal.objetodetalle));
            //console.log(thisLocal.objetodetalle);
            return this.pajaroArray;
        }
    },


    mounted() {
        let thisLocal = this;
        console.log('carga automatica');
        axios.get('https://aves.ninjas.cl/api/birds')
        .then( function( response ) {
            //console.log(response.data)
            thisLocal.posts = response.data
            console.log("posteos1",thisLocal.posts[0])
            thisLocal.getDetail(thisLocal.posts[0])  

        })


    }
}
</script>

<style scoped>
    .container{
        padding: 10px;
        border: 1px solid black;
        border-radius: 25px;
        background-color: white;
    }

    .grid{
        display: grid;
        grid-template-columns: 23% 77%;
        gap: 10px;
        padding-top: 30px;
        padding-right: 30px;
    }

    .caja2{
     /* background-color: turquoise; */
     justify-self: start;
    }

    .caja1{
     /* background-color:wheat; */
    }

    .caja-imagen{
        height: 70px;
        width: 70px;
        /* border: 1px solid black; */
        justify-self: start;
    }

    .caja-imagen-detalle{
        height: 160px;
        width: 230px;
        margin-left: 10px;
    }

    .nombre-latin{
        font-family:'Times New Roman', Times, serif;
        margin-left: 10px;
        
    }

    .nombre-pajaro{
        font-weight: bold;
        display: flex;
        justify-content: center;
        align-items: center;
        margin-left: 10px;
    }

    .mapa{
        background-color:#F2F2F2;
        border-radius: 25px;
    }


    .imagen{
        height: 1100px;
    }

    .lista-de-pajaros{
        display: flex;
        flex-flow: row wrap;

    }

    .titulo-detalle{
        display: flex;
        align-items: flex-start;
        justify-content: start;
        flex-direction: column;
    }

    .pajaro-detail{
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
    }

    .detalle{
        text-align: justify;
        margin-left: 20px;
    }

</style>
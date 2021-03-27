<template>
  <div>
     <div> <v-text-field  class="shrink" v-model="producto"></v-text-field>
      <v-btn color="black" dark @click="iniciarOffset">Buscar</v-btn></div> 
         
      <div><v-btn color="red"  @click="devolverP">atras</v-btn>
      <v-btn color="blue"  @click="siguientesP"> siguiente </v-btn></div>
      
    <v-data-table :headers="headers" :items="itm[3]" :items-per-page="50" class="elevation-1"
                    disable-pagination>
                    <template v-slot:[`itm.thumbnail`]="{ itm }">
                        <div class="p-2">
                            <v-img :src="itm.thumbnail" :alt="itm.thumbnail" height="100px"></v-img>
                        </div>
                    </template>
                    <template v-slot:[`itm.detalle`]="{ itm }">
                        <v-icon class="ml-2" @click="detalle(itm.id)">
                            mdi-eye
                        </v-icon>
                    </template>
                </v-data-table>
  </div>
 
    

</template>

<script>
import axios from "axios";

export default {
  name: "IBusqueda",
  data() {
    return {
        headers: [
                    
                    {
                        text: 'Nombre',
                        value: 'title'
                    },
                    {
                        text: 'Precio',
                        value: 'price'
                    },
                    
                    {
                        text: 'Ciudad',
                        value: 'address.state_name'
                    },
                    {
                        text: 'Ver',
                        value: 'detalle'
                    },{
                        text: 'Imagen',
                        value: 'thumbnail'
                    }
                ],
      producto: "",
      limit: 50,
      offset: 0,
      totalP : 0,
      itm:[]
    };
  },

  methods: {
    
    async busqueda() {
        
            await axios
        .get("https://api.mercadolibre.com/sites/MCO/search?q=" + this.producto + "&offset=" + this.offset)
        .then((response) =>{this.emitResponse(response)
            console.log(response.data)
             this.itm=Object.values(response.data)
        } 
        );
        
        
    },
    iniciarOffset() {
        this.offset = 0;
        this.busqueda();
    },
    siguientesP() {
        if (this.offset+this.limit < this.totalP) {
            this.offset+= this.limit;
            this.busqueda();
            console.log(this.offset);
        }
    },
    devolverP() {
        if(this.offset == 0) {
            return;
        } else {
            this.offset-= this.limit;
            this.busqueda();
        }
    },
     emitResponse(response) {
        if (response.data.results.length != 0) {
            this.totalP = response.data.paging.total;
            this.$emit("searchResult", response);
        }
    }
  }
}
</script>

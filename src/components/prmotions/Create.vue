<template>
    <div class="min-h-screen bg-gray-100 flex items-center">
        <div class="container mx-auto max-w-md shadow-md hover:shadow-lg transition duration-300">
            <div class="py-12 p-10 bg-white rounded-xl">
                <div class="mb-6">
                    <label class="mr-4 text-gray-700 font-bold inline-block mb-2">Descripción:</label>
                    <input type="text" @blur="validate('description')" :class="validateClass.description" v-model="promotion.description" />
                </div>
                <div class="mb-6">
                    <label class="mr-4 text-gray-700 font-bold inline-block mb-2" >Porcentaje:</label>
                    <input type="number" step="0.1" @blur="validate('percentaje')" :class="validateClass.percentaje" v-model="promotion.percentaje"  />
                </div>
                <div class="mb-6">
                    <label class="mr-4 text-gray-700 font-bold inline-block mb-2" >Fecha de Inicio:</label>
                    <input type="date" @blur="validate('startDate')" :class="validateClass.startDate" v-model="promotion.startDate" />
                </div>
                <div class="mb-6">
                    <label class="mr-4 text-gray-700 font-bold inline-block mb-2" >Fecha de Final:</label>
                    <input type="date" @blur="validate('finishDate')" :class="validateClass.finishDate" v-model="promotion.finishDate" />
                </div>
                <button class="w-full mt-6 text-indigo-50 font-bold bg-indigo-600 py-3 rounded-md hover:bg-indigo-500 transition duration-300" @click="save">Guardar</button>
            </div>
        </div>
    </div>
</template>

<script>
import { ref } from 'vue'
import axios from 'axios'
export default {
    setup(){
        let promotion = ref({
            description: '',
            percentaje: 0.0,
            startDate: '',
            finishDate:''
        })
        let validateClass = ref({
             description: "border bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded",
            percentaje: "border bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded",
            startDate: "border bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded",
            finishDate: "border bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded",
        })
        const save = ()=>{
            //promotion.value.price = parseFloat(promotion.value.price)
            axios.post(`${process.env.VUE_APP_HOST}/v1/promotion`,promotion.value)
                .then((response) =>{
                    if(response.data.error){
                        alert("No puede existir una promoción el mismo rango de fechas")
                    }else{
                        alert("Registro Guardado")
                        window.location.href = "/#/promociones";
                    }
                })
                .catch((err) =>{
                    console.log(err)
                    alert("Error al Guardar Promocion")
                });
        }

        const validate = (flag)=>{
            if( flag==="description" && (promotion.value.description === "" || promotion.value.description.lenght > 120)){
                validateClass.value.description = "border border-red-300 bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded"
            }else if(flag==="description"){
                validateClass.value.description = "border bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded"
            }
            if( flag==="percentaje" && (promotion.value.percentaje === "" || promotion.value.percentaje < 0.1)){
                validateClass.value.percentaje = "border border-red-300 bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded"
            }else if(flag==="percentaje"){
                validateClass.value.percentaje = "border bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded"
            }
            if( flag==="startDate" && (promotion.value.startDate === "")){
                validateClass.value.startDate = "border border-red-300 bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded"
            }else if(flag==="startDate"){
                validateClass.value.startDate = "border bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded"
            }
             if( flag==="finishDate" && (promotion.value.finishDate === "")){
                validateClass.value.finishDate = "border border-red-300 bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded"
            }else if(flag==="finishDate"){
                validateClass.value.finishDate = "border bg-gray-100 py-2 px-4 w-96 outline-none focus:ring-2 focus:ring-indigo-400 rounded"
            }
        }

        return {
            promotion,
            save,
            validateClass,
            validate,
        }
    }
}
</script>

<style>

</style>
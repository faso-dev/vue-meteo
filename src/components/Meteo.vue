<template>
    <v-card
            class="mx-auto"
            max-width="400"
    >
        <v-list-item two-line>
            <v-list-item-content>
                <v-list-item-title style="font-size: 25px; font-weight: bolder" class="text--primary font-weight-bold pa-2 pt-2">
                    FASO-METEO | {{ city.toUpperCase() }}
                </v-list-item-title>
                <v-list-item-subtitle>
                   <v-row>
                       <ValidationObserver v-slot="{ handleSubmit }">
                       <v-col cols="12">
                           <ValidationProvider name="ville" rules="required|min:2|max:30" v-slot="{ errors }">
                           <v-text-field
                                   style="font-size: 20px"
                                   class="p-2 font-weight-light"
                                   v-model="city"
                                   type="text"
                                   @keydown="errors = []"
                                   placeholder="Entrer votre ville..."
                           />
                               <span class="red--text">{{ errors[0] }}</span>
                           </ValidationProvider>
                       </v-col>
                       <v-col cols="12">
                           <v-btn class="primary" @click="handleSubmit(loadMeteoData)">Voir</v-btn>
                       </v-col>
                       </ValidationObserver>
                   </v-row>
                </v-list-item-subtitle>
            </v-list-item-content>
        </v-list-item>

        <v-card-text>
            <v-row v-model="meteoData" align="center">
                <v-col class="display-3" cols="6">
                    {{ meteoData.temp }}&deg;C
                </v-col>
                <v-col cols="6">
                    <v-img
                            src="https://cdn.vuetifyjs.com/images/cards/sun.png"
                            alt="Sunny image"
                            width="92"
                    />
                </v-col>
            </v-row>
        </v-card-text>
        <v-container>
            <v-content>
                <v-card-text>
                    Cette application vous donnes
                    les données méteorologiques de
                    la ville de {{ city.toUpperCase() }}
                </v-card-text>
            </v-content>
        </v-container>
        <v-list-item>
            <v-list-item-icon>
                <v-icon>mdi-send</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>Vitesse vent : {{ meteoData.speed }} km/h</v-list-item-subtitle>
        </v-list-item>

        <v-list-item>
            <v-list-item-icon>
                <v-icon>mdi-cloud-download</v-icon>
            </v-list-item-icon>
            <v-list-item-subtitle>Humidité : {{ meteoData.humidity }} %</v-list-item-subtitle>
        </v-list-item>
        <v-divider/>
        <v-container>
            <v-content class="text-center pa-5">
                Power by faso-dev | &copy;2020-2021
               <v-content>
                   <v-img
                           class="mt-2"
                           src="../assets/img/faso-dev.png"
                           alt="FASO-DEV LOGO"
                           style="height: 100px!important;"
                   />
               </v-content>
            </v-content>
        </v-container>
    </v-card>
</template>

<script>
    export default {
        name: "Meteo",
        data(){
            return {
                city : 'Ouagadougou',
                icon : '',
                meteoData : {
                    temp : '',
                    name : '',
                    country : '',
                    city : '',
                    main : '',
                    description : '',
                    speed : '',
                    humidity : '',
                },
            }
        },
        created: function () {
            this.$nextTick(this.loadMeteoData)
        },
        methods : {
            loadMeteoData : function () {
                var vm = this;
                fetch(`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=3053291ce6abf96a5d1c4bc46647d3d6`,{
                    method : 'GET'
                }).then(response => response.json())
                .then(data => {
                    vm.meteoData.temp = data.main.temp;
                    vm.meteoData.country = data.sys.country;
                    vm.meteoData.city = data.sys.name;
                    vm.meteoData.main = data.weather.main;
                    vm.meteoData.description = data.weather.description;
                    vm.meteoData.speed = data.wind.speed;
                    vm.meteoData.humidity = data.main.humidity;

                })

            },

        }
    }

</script>

<style scoped>

</style>

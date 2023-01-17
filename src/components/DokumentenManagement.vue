<template>

    <v-row no-gutters class="bg-grey">
        <v-col cols="3">
            <v-sheet class="pa-2 ma-2">
                <v-card class="mx-auto" max-width="344" variant="outlined">
                    <v-card-item>
                        <div>
                            <div class="text-overline ma-0">
                                Belegung
                            </div>

                            <v-select density="compact" label="Abteilung" :items="['Neurologie']"></v-select>
                            <v-select density="compact" label="Zweig" :items="['ALLe']"></v-select>
                            <v-select density="compact" label="Station"
                                :items="['Ebene1', 'Ebene2', 'Ebene3', 'Ebene4', 'Ebene5', 'Ebene6']"></v-select>

                        </div>
                    </v-card-item>

                </v-card>

                <v-card class="mx-auto" max-width="344" variant="outlined">
                    <v-card-item>
                        <div>
                            <div class="text-overline ma-0">
                                Patientendaten
                            </div>
                            <v-text-field density="compact" label="Name"></v-text-field>
                            <v-text-field density="compact" label="Vorname"></v-text-field>
                            <v-text-field density="compact" label="Geb. Datum"></v-text-field>
                        </div>
                    </v-card-item>

                </v-card>

                <v-card class="mx-auto" max-width="344" variant="outlined">
                    <v-card-item>
                        <div>
                            <div class="text-overline ma-0">
                                Dokumente
                            </div>
                            <v-select density="compact" label="Zeitraum" :items="['Letzten Monat']"></v-select>
                            <v-card class="mx-auto" max-width="344" variant="outlined">
                                <v-card-item>
                                    <div>
                                        <div class="text-overline ma-0">
                                            Arztbrief
                                        </div>
                                        <v-radio-group>
                                            <v-radio density="compact" label="keine" value="1"></v-radio>
                                            <v-radio density="compact" label="angelegte Arztbriefe" value="2"></v-radio>
                                            <v-radio density="compact" label="versendete nicht fixierte Arztbriefe"
                                                value="3"></v-radio>
                                            <v-radio density="compact" label="fixierte Arztbriefe" value="4"></v-radio>
                                            <v-radio density="compact" label="alle" value="5"></v-radio>
                                        </v-radio-group>
                                    </div>
                                </v-card-item>
                            </v-card>

                            <v-btn @click="searchDocuments">Suche</v-btn>

                        </div>
                    </v-card-item>

                </v-card>
            </v-sheet>
        </v-col>

        <v-col cols="9">
            <v-data-table 
            v-model:items-per-page="itemsPerPage" 
            :headers="headers" 
            :items="documents" 
            item-value="name"
            class="elevation-1"
            @click:row="selectVorlage"
            >
            </v-data-table>
            <div class="mb-6 bg-purple-lighten-5">
                <v-container fluid>
                    <v-row class="d-flex justify-center ">
                        <v-col cols="12" sm="3" md="3">
                            <v-checkbox v-model="isFiltered" label="Filter" color="blue-darken-3"
                                hide-details></v-checkbox>
                        </v-col>
                        <v-col cols="12" sm="3" md="3">
                            <v-checkbox v-model="isUmlauf" label="Umlaufinformationen" color="blue-darken-3"
                                hide-details></v-checkbox>

                        </v-col>
                    </v-row></v-container>
            </div>
        </v-col>

    </v-row>
</template>

<script>
// import CkEditor4 from './CkEditor4.vue';
import { ref } from "vue";

export default {
    name: "DokumentenManagement",
    components: {
        // CkEditor4
    },
    setup() {
        const  documents =ref([
                { dokument: "Ergänzender Enthlassungsbrief Wf", v: "", erstelldatum: "26.09.2022", patient: "Hering, Detlef - 08.10.1949", vorlage: "Brief", Belegung: "Neuro-Akut-Ebene1", zustand: "angelegt", uDatum: "16.06.2022", uStatus: "Neu", unterschrift1: "Simona-Gabri...", unterschrift2: "", unterschrift3: "" },
                { dokument: "Notw. für Begleitperson", v: "", erstelldatum: "13.09.2022", patient: "Hering, Detlef - 08.10.1949", vorlage: "Brief", Belegung: "Neuro-Akut-Ebene1", zustand: "angelegt", uDatum: "16.06.2022", uStatus: "Zur Unterschrift 1", unterschrift1: "Prof. Dr. me....", unterschrift2: "Simona-Gabri...", unterschrift3: "" },
                { dokument: "Aktuelle Medikation", v: "", erstelldatum: "14.09.2022", patient: "Raida, Klaus - 01.11.1955", vorlage: "Brief", Belegung: "Neuro-Akut-Ebene1", zustand: "angelegt", uDatum: "16.06.2022", uStatus: "Neu", unterschrift1: "M. Canelo Är...", unterschrift2: "", unterschrift3: "" },
                { dokument: "Ergänzender Entlassungsbrief WF", v: "", erstelldatum: "06.10.2022", patient: "Schätti, Thomas - 04.09.1958", vorlage: "Brief", Belegung: "Neuro-Akut-Ebene1", zustand: "angelegt", uDatum: "16.06.2022", uStatus: "Zur Formatierung", unterschrift1: "Simona-Gabri...", unterschrift2: "", unterschrift3: "" },
                {
                    dokument: "Liegebescheinigung mit Diagnosen", v: "", erstelldatum: "14.09.2022",
                    patient: "Roede, Heiko - 16.11.1962", vorlage: "Brief", Belegung: "Neuro-Akut-Ebene1",
                    zustand: "angelegt", uDatum: "16.06.2022", uStatus: "Neu", unterschrift1: "",
                    unterschrift2: "Simona-Gabri...", unterschrift3: ""
                },
            ])


        function selectVorlage(value) {
            const tr = value.target.parentNode;
            console.log(tr);
            console.log("selectVorlage", value);
        }
        function searchDocuments() {
            
            documents.value = documents.value.filter(a => a.dokument.includes('zender'))

            //console.log(documents);
            
            console.log('search docus');
            console.log(documents);
        }


        return { selectVorlage, searchDocuments, documents }

    },
    data() {
        return {
            isFiltered: false,
            isUmlauf: false,
           
            itemsPerPage: 10,
            headers: [
                {
                    title: 'dokument',
                    align: 'start',
                    sortable: true,
                    key: 'dokument',
                    width: '20%'
                },
                // { title: 'v', align: 'end', key: 'v' },
                { title: 'erstelldatum', align: 'start', key: 'erstelldatum', width: '10%' },
                { title: 'patient', align: 'start', key: 'patient', width: '30%' },
                { title: 'vorlage', align: 'start', key: 'vorlage', width: '5%' },
                { title: 'Belegung', align: 'start', key: 'Belegung', width: '17%' },
                { title: 'zustand', align: 'start', key: 'zustand', width: '10%' },
                // { title: 'uDatum', align: 'end', key: 'uDatum' , width: '20%'},
                // { title: 'uStatus', align: 'end', key: 'uStatus' , width: '20%'},
                { title: 'unterschrift1', align: 'start', key: 'unterschrift1', width: '10%' },
                { title: 'unterschrift2', align: 'start', key: 'unterschrift2', width: '10%' },
                { title: 'unterschrift3', align: 'start', key: 'unterschrift3', width: '10%' },

            ],

        }
    },



}
</script>
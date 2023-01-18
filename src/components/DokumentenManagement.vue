<template>

    <v-row no-gutters class="bg-grey-lighten-1 d-flex justify-space-end">
        <v-col cols="3" style="max-width: 320px;">
            <v-sheet class="pa-2 ma-2">
                <v-card class="mx-auto" max-width="344" variant="outlined">
                    <v-card-item>
                        <div>
                            <div class="text-overline ma-0">
                                Belegung
                            </div>
                            <!-- :hint="`${selectedCategorie.name}, ${selectedCategorie.abbr}`"  -->
                            <v-select v-model="selectedAbteilung" :items="abteilungs" :hint="selectedAbteilung.abbr"
                                density="compact" item-title="name" item-value="abbr" label="Select" persistent-hint
                                return-object single-line>
                            </v-select>
                            <v-select v-model="selectedZweig" :items="zweigs" density="compact" item-title="name"
                                item-value="abbr" label="Select" persistent-hint return-object single-line>
                            </v-select>
                            <v-select v-model="selectedStation" :items="stations" density="compact" item-title="name"
                                item-value="abbr" label="Select" persistent-hint return-object single-line>
                            </v-select>

                        </div>
                    </v-card-item>

                </v-card>

                <v-card class="mx-auto" max-width="344" variant="outlined">
                    <v-card-item>
                        <div>
                            <div class="text-overline ma-0">
                                Patientendaten
                            </div>
                            <v-text-field density="compact" label="Name" v-model="selectdPatient.name"></v-text-field>
                            <v-text-field density="compact" label="Vorname"
                                v-model="selectdPatient.firstName"></v-text-field>
                            <v-text-field density="compact" label="Geb. Datum"
                                v-model="selectdPatient.birthDay"></v-text-field>
                        </div>
                    </v-card-item>

                </v-card>

                <v-card class="mx-auto" max-width="344" variant="outlined">
                    <v-card-item>
                        <div>
                            <div class="text-overline ma-0">
                                Dokumente
                            </div>
                            <v-select v-model="selectedZeitraum" :items="zeitraums"
                                :hint="'ab: ' + selectedZeitraum.abbr" density="compact" item-title="name"
                                item-value="abbr" label="Zeitraum" persistent-hint return-object single-line>
                            </v-select>

                            <v-card class="mx-auto" max-width="344" variant="outlined">
                                <v-card-item>
                                    <div>
                                        <div class="text-overline ma-0">
                                            Arztbrief {{  radios}}
                                        </div>
                                        <v-radio-group v-model="radios">

                                            <v-radio density="compact" label="keine" value="keine"></v-radio>
                                            <v-radio density="compact" label="angelegte Arztbriefe" value="angelegt"></v-radio>
                                            <v-radio density="compact" label="versendete nicht fixierte Arztbriefe"
                                                value="3"></v-radio>
                                            <v-radio density="compact" label="fixierte Arztbriefe" value="fixierte"></v-radio>
                                            <v-radio density="compact" label="alle" value=""></v-radio>
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

        <v-col cols="9" style="max-width: 100;" class="pt-2">

            <v-data-table v-model:items-per-page="itemsPerPage" :headers="headers" :items="filteredDocuments"
                item-value="name" class="elevation-1" @click:row="selectVorlage">
            </v-data-table>
            <div class="mb-6 bg-light-blue-lighten-5">
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
        const radios = ref('');
        
        const selectdPatient = ref({ name: '', firstName: '', birthDay: '' })
        const selectedAbteilung = ref({ name: 'Alle', abbr: '' });
        const abteilungs = ref([
            { name: 'Alle', abbr: '' },
            { name: 'Neurologie', abbr: 'Neur' },
            { name: 'Radiologie', abbr: 'Radi' },
            { name: 'Gastroenterologie', abbr: 'Gast' },
        ]);

        const selectedZweig = ref({ name: 'Alle', abbr: '' });
        const zweigs = ref([
            { name: 'Alle', abbr: '' },
            { name: 'Akut', abbr: 'Akut' },
            { name: 'Notaufnahmen', abbr: 'Not' },

        ]);

        const selectedStation = ref({ name: 'Alle', abbr: '' });
        const stations = ref([
            { name: 'Alle', abbr: '' },
            { name: 'Ebene1', abbr: 'Ebene1' },
            { name: 'Ebene2', abbr: 'Ebene2' },
            { name: 'Ebene3', abbr: 'Ebene3' },
            { name: 'Ebene4', abbr: 'Ebene4' },
            { name: 'Ebene5', abbr: 'Ebene5' },
            { name: 'Ebene6', abbr: 'Ebene6' },
        ]);

        const selectedZeitraum = ref({ name: 'Alle', abbr: '' });
        const zeitraums = ref([
            { name: 'Alle', abbr: '' },
            { name: 'Letzten Monat', abbr: subtractMonths(new Date(), 1) },
            { name: 'Letzten Quartal', abbr: subtractMonths(new Date(), 3) },
            { name: 'Letzten Habjahr', abbr: subtractMonths(new Date(), 6) },
            { name: 'Letzten Jahr', abbr: subtractMonths(new Date(), 12) },
        ]);



        const filteredDocuments = ref([]);
        const documents = ref([
            { dokument: "Ergänzender Enthlassungsbrief Wf", v: "", erstelldatum: "26.09.2022", patient: "Hering, Detlef - 08.10.1949", vorlage: "Brief", Belegung: "Neuro-Akut-Ebene1", zustand: "angelegt", uDatum: "16.06.2022", uStatus: "Neu", unterschrift1: "Simona-Gabri...", unterschrift2: "", unterschrift3: "" },
            { dokument: "Notw. für Begleitperson", v: "", erstelldatum: "13.09.2022", patient: "Hering, Detlef - 08.10.1949", vorlage: "Brief", Belegung: "Neuro-Akut-Ebene1", zustand: "angelegt", uDatum: "16.06.2022", uStatus: "Zur Unterschrift 1", unterschrift1: "Prof. Dr. me....", unterschrift2: "Simona-Gabri...", unterschrift3: "" },
            { dokument: "Aktuelle Medikation", v: "", erstelldatum: "14.09.2022", patient: "Raida, Klaus - 01.11.1955", vorlage: "Brief", Belegung: "Neuro-Akut-Ebene1", zustand: "angelegt", uDatum: "16.06.2022", uStatus: "Neu", unterschrift1: "M. Canelo Är...", unterschrift2: "", unterschrift3: "" },
            { dokument: "Ergänzender Entlassungsbrief WF", v: "", erstelldatum: "06.10.2022", patient: "Schätti, Thomas - 04.09.1958", vorlage: "Brief", Belegung: "Neuro-Akut-Ebene1", zustand: "angelegt", uDatum: "16.06.2022", uStatus: "Zur Formatierung", unterschrift1: "Simona-Gabri...", unterschrift2: "", unterschrift3: "" },
            {
                dokument: "Liegebescheinigung mit Diagnosen", v: "", erstelldatum: "14.09.2022",
                patient: "Roede, Heiko - 16.11.1962", vorlage: "Brief", Belegung: "Radi-Not-Ebene5",
                zustand: "", uDatum: "16.06.2022", uStatus: "Neu", unterschrift1: "",
                unterschrift2: "Simona-Gabri...", unterschrift3: ""
            },
            {
                dokument: "Liegebescheinigung mit Diagnosen", v: "", erstelldatum: "14.09.2022",
                patient: "Roede, Heiko - 16.11.1962", vorlage: "Brief", Belegung: "Radi-Akut-Ebene1",
                zustand: "fixierte", uDatum: "16.06.2022", uStatus: "Neu", unterschrift1: "",
                unterschrift2: "Simona-Gabri...", unterschrift3: ""
            },
        ]);

        function subtractMonths(date, months) {
            //console.log(date, months);
            console.log(date, months);
            
            date.setMonth(date.getMonth() - months);

            // const formatedDate = date.toLocaleString('de-DE',{ year: "numeric", month: "numeric", day: "numeric"});
            const formatedDate = date.toLocaleString('en-US',{ year: "numeric", month: "numeric", day: "numeric"});
            

            return  formatedDate;
        }


        function selectVorlage(value) {
            // const tr = value.target.parentNode;
            // console.log(tr);
            console.log("selectVorlage", value);
        }
        function searchDocuments() {
            console.log(new Date(selectedZeitraum.value.abbr) - new Date());
            filteredDocuments.value = documents.value.filter(a => 
            a.Belegung.includes(selectedAbteilung.value.abbr) &&
            a.Belegung.includes(selectedZweig.value.abbr) &&
            a.Belegung.includes(selectedStation.value.abbr) &&
            a.patient.includes(selectdPatient.value.name) &&
            a.patient.includes(selectdPatient.value.firstName) &&
            a.patient.includes(selectdPatient.value.birthDay) 
            //&& compareDates(a.erstelldatum, selectedZeitraum.value.abbr)
            && (radios.value === 'keine') ? a.zustand === '' : a.zustand.includes(radios.value)
            );
        }
        // function compareDates(date1, date2) {
            
        //     console.log(typeof(date1), Date.parse(date1));
        //     console.log(typeof(date2), typeof(Date.parse(date2)));

        //     const date1formated = subtractMonths(subtractMonths, 0)
        //     console.log(new Date(date1formated));
        //     return true         
        // }


        return { subtractMonths, selectVorlage, searchDocuments, documents, abteilungs, selectedAbteilung, filteredDocuments, zweigs, selectedZweig, stations, selectedStation, selectdPatient, zeitraums, selectedZeitraum, radios }

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
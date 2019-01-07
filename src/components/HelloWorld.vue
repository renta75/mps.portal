<template>
  <v-container>
    <v-card class="v_card">
      <v-form ref="form" v-model="valid" lazy-validation v-if="page===1">
        <h1 style="text-align:center;">ZAHTJEV ZA POTPORU</h1>
        <h2
          style="text-align:center;"
        >Natječaj za dodjelu potpore u okviru mjere I.1. „Inovacije“ („Narodne novine“, broj 57/2017)</h2>
        <v-text-field
          v-model="name"
          :rules="nameRules"
          :counter="255"
          label="Ime"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="OIB"
          :counter="11"
          label="OIB (samo u slučaju fizičke osobe)"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field v-model="city" :counter="50" label="Mjesto" required class="text_field"></v-text-field>
        <v-text-field
          v-model="ZIP"
          :counter="50"
          label="Poštanski broj"
          required
          class="text_field"
        ></v-text-field>
        <v-select
          v-model="county"
          :items="counties"
          :rules="[v => !!v || 'Polje je obavezno']"
          label="Županija"
          required
          class="text_field"
        ></v-select>
        <v-text-field
          v-model="email"
          :rules="emailRules"
          label="E-mail"
          required
          class="text_field"
        ></v-text-field>
        <br>
        <br>
        <hr>
        <br>
        <br>
        <v-radio-group v-model="tipKorisnika" label="Tip korisnika">
          <v-radio :key="gospodarskiRibolov" :label="'Gospodarski ribolov'" :value="0"></v-radio>
          <v-radio
            :key="preradaProizvodaRibarstva"
            :label="'Prerada proizvoda ribarstva'"
            :value="1"
          ></v-radio>
          <v-radio :key="zajedničkiKorisnik" :label="'Zajednički korisnik'" :value="2"></v-radio>
          <v-radio :key="znanstvenoTijelo" :label="'Znanstveno tijelo'" :value="3"></v-radio>
        </v-radio-group>
        <v-radio-group
          v-model="zaGospodraskiRibolov"
          v-if="tipKorisnika===0"
          label="Za gospodarski riblov"
        >
          <v-radio :key="vlasnikPovlastice" :label="'Vlasnik povlastice'" :value="0"></v-radio>
          <v-radio :key="ovlastenikPovlastice" :label="'Ovlaštenik povlastice'" :value="1"></v-radio>
        </v-radio-group>
        <v-radio-group
          v-model="pravniOblikKorisnika"
          v-if="tipKorisnika===1"
          label="Pravni oblik korisnika"
        >
          <v-radio :key="fizickaOsoba" :label="'Fizička osoba'" :value="0"></v-radio>
          <v-radio :key="obrt" :label="'Obrt'" :value="1"></v-radio>
          <v-radio :key="trgovackoDrustvo" :label="'Trgovačko društvo'" :value="2"></v-radio>
          <v-radio :key="ustanova" :label="'Ustanova'" :value="3"></v-radio>
        </v-radio-group>

        <v-radio-group v-model="zaObrt" v-if="pravniOblikKorisnika===1" label="Za obrt">
          <v-radio :key="obveznikPorezaNaDohodak" :label="'Obveznik poreza na dohodak'" :value="0"></v-radio>
          <v-radio :key="obveznikPorezaNaDobit" :label="'Obveznik poreza na dobit'" :value="1"></v-radio>
        </v-radio-group>

        <v-radio-group
          v-model="zaTrgovackoDrustvo"
          v-if="pravniOblikKorisnika===2"
          label="Za trgovačko društvo"
        >
          <v-radio :key="doo" :label="'d.o.o.'" :value="0"></v-radio>
          <v-radio :key="jdoo" :label="'j.d.o.o.'" :value="1"></v-radio>
          <v-radio :key="dd" :label="'d.d.'" :value="2"></v-radio>
        </v-radio-group>
        <v-radio-group
          v-model="zaZajednickiKorisnik"
          v-if="tipKorisnika===2"
          label="Za zajednički korisnik"
        >
          <v-radio :key="zadruga" :label="'Ribarska zadruga'" :value="0"></v-radio>
          <v-radio
            :key="organizacija"
            :label="'Organizacija proizvođaća/udruženje organizacija proizvođaća / međusektorska organizacija '"
            :value="1"
          ></v-radio>
          <v-radio
            :key="javnoTijelo"
            :label="'Javno tijelo utemljeno zakonom koje predstavlja subjekte u ribarstvu '"
            :value="2"
          ></v-radio>
        </v-radio-group>
        <v-radio-group v-model="korisnikObveznikPDVa" label="Korisnik obveznik PDV-a">
          <v-radio :key="da" :label="'Da'" :value="0"></v-radio>
          <v-radio :key="ne" :label="'ne'" :value="1"></v-radio>
        </v-radio-group>

        <v-radio-group
          v-model="korisnikObveznikPostupkaJavneNabave"
          label="Korisnik obveznik postupka javne nabave"
        >
          <v-radio :key="da" :label="'Da'" :value="0"></v-radio>
          <v-radio :key="ne" :label="'ne'" :value="1"></v-radio>
        </v-radio-group>

        <v-radio-group v-model="velicinaPoduzeca" label="Veličina poduzeća">
          <v-radio :key="mikro" :label="'Mikro'" :value="0"></v-radio>
          <v-radio :key="malo" :label="'Malo'" :value="1"></v-radio>
          <v-radio :key="srednje" :label="'Srednje'" :value="2"></v-radio>
          <v-radio :key="veliko" :label="'Veliko'" :value="3"></v-radio>
        </v-radio-group>

        <v-checkbox
          v-model="checkbox"
          :rules="[v => !!v || 'Morate se složiti da bi nastavili!']"
          label="Slažete li se?"
          required
        ></v-checkbox>
        </v-form>

        <v-form ref="form" v-model="valid" lazy-validation v-if="page===2">
        <h1 style="text-align:center;">SAMO ZA OBRTE</h1>
        <v-text-field
          v-model="vlasnikObrtaImePrezime"
          :counter="255"
          label="Ime i prezime vlasnika obrta"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="oibNositeljaObrta"
          :counter="11"
          label="OIB nositelja obrta"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="imePrezimeOdgovorneOsobeObrt"
          :counter="255"
          label="Ime i prezime odgovorne osobe"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="oibOdgovorneOsobeObrt"
          :counter="11"
          label="OIB odgovorne osobe"
          required
          class="text_field"
        ></v-text-field>
        <v-btn @click="clear" class="clean-btn">očisti</v-btn>
      </v-form>

      <v-form ref="form" v-model="valid" lazy-validation v-if="page===3">
        <h1 style="text-align:center;">SAMO ZA PRAVNE OSOBE</h1>
        <v-text-field
          v-model="imePrezimeVlasnikaDirektora"
          :counter="255"
          label="Ime i prezime vlasnika ili direktora"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="oibPravneOsobe"
          :counter="11"
          label="OIB pravne osobe"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="imePrezimeOdgovorneOsobePravna"
          :counter="255"
          label="Ime i prezime odgovorne osobe"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="oibOdgovorneOsobePravna"
          :counter="11"
          label="OIB odgovorne osobe"
          required
          class="text_field"
        ></v-text-field>
        <v-btn @click="clear" class="clean-btn">očisti</v-btn>
      </v-form>

      <v-form ref="form" v-model="valid" lazy-validation v-if="page===4">
        <h1 style="text-align:center;">SAMO ZA JAVNA TIJELA/USTANOVE</h1>
        <v-text-field
          v-model="oibJavnogTijela"
          :counter="11"
          label="OIB javnog tijela"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="imePrezimeOdgovorneOsobeJavna"
          :counter="255"
          label="Ime i prezime odgovorne osobe"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="oibOdgovorneOsobeJavna"
          :counter="11"
          label="OIB odgovorne osobe"
          required
          class="text_field"
        ></v-text-field>
        <v-btn @click="clear" class="clean-btn">očisti</v-btn>
      </v-form>

      <v-form ref="form" v-model="valid" lazy-validation v-if="page===5">
        <h1 style="text-align:center;">2. PODACI O KONTAKT OSOBI</h1>
        <v-text-field
          v-model="kontaktIme"
          :counter="255"
          label="Ime"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="kontaktPrezime"
          :counter="255"
          label="Prezime"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="kontaktTelFax"
          :counter="255"
          label="Telefon/faks"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="kontaktMobitel"
          :counter="255"
          label="Mobitel"
          required
          class="text_field"
        ></v-text-field>
        <v-text-field
          v-model="kontaktEmail"
          :counter="255"
          label="Email"
          required
          class="text_field"
        ></v-text-field>
        <v-btn @click="clear" class="clean-btn">očisti</v-btn>
      </v-form>

      <v-form ref="form" v-model="valid" lazy-validation v-if="page===6">
        <h1 style="text-align:center;">3. PODACI O VLASNICIMA/SUVLASNICIMA PLOVILA</h1>
        <h2
          style="text-align:center;"
        >(ispunjava se isključivo ako se operacija odnosi na fizičko ulaganje u plovilo i to u slučajevima kada Zahtjev za potporu podnosi vlasnik plovila, a nije jedini vlasnik te kada Zahtjev za potporu podnosi ovlaštenik povlastice, a koji nije ujedno i vlasnik ribarskog plovila. Ako su operacijom obuhvaćena fizička ulaganja u više ribarskih plovila, po potrebi dodati nove retke i navesti podatke za svako plovilo u slučaju više vlasnika/različitog vlasnika od ovlaštenika)</h2>

        <v-text-field
          v-model="cfrPlovila1"
          :counter="255"
          label="CFR plovila"
          required
          class="text_field"
        ></v-text-field>

        <div>
          <v-toolbar flat color="white">
            <v-toolbar-title>Suvlasnici plovila</v-toolbar-title>
            <v-divider class="mx-2" inset vertical></v-divider>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
              <v-btn slot="activator" color="primary" dark class="mb-2">Novi suvlasnik</v-btn>
              <v-card>
                <v-card-title>
                  <span class="headline">{{ formTitlePlovilo }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container grid-list-md>
                    <v-layout wrap>
                      <v-flex xs12 sm6 md4>
                        <v-text-field v-model="editedItemPlovilo.rbr" label="Rbr."></v-text-field>
                      </v-flex>
                      <v-flex xs12 sm6 md4>
                        <v-text-field v-model="editedItemPlovilo.imePrezime" label="Ime i prezime"></v-text-field>
                      </v-flex>
                      <v-flex xs12 sm6 md4>
                        <v-text-field v-model="editedItemPlovilo.oib" label="OIB"></v-text-field>
                      </v-flex>
                    </v-layout>
                  </v-container>
                </v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" flat @click="closePlovilo">Odustani</v-btn>
                  <v-btn color="blue darken-1" flat @click="savePlovilo">Spremi</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
          <v-data-table :headers="headersPlovilo" :items="plovilo" class="elevation-1">
            <template slot="items" slot-scope="props">
              <td class="text-xs-left" >{{ props.item.rbr }}</td>
              <td class="text-xs-left">{{ props.item.imePrezime }}</td>
              <td class="text-xs-left" >{{ props.item.oib }}</td>
              <td class="justify-center layout px-0">
                <v-icon small class="mr-2" @click="editItemPlovilo(props.item)">edit</v-icon>
                <v-icon small @click="deleteItemPlovilo(props.item)">delete</v-icon>
              </td>
            </template>
            <template slot="no-data">
              
            </template>
          </v-data-table>
        </div>
      </v-form>

      <v-form ref="form" v-model="valid" lazy-validation v-if="page===7">
        <h1 style="text-align:center;">7. LISTA PRIHVATLJIVIH TROŠKOVA U OKVIRU OPERACIJE</h1>
        <div>
          <v-toolbar flat color="white">
            <v-toolbar-title>Lista prihvatljivih troškova</v-toolbar-title>
            <v-divider class="mx-2" inset vertical></v-divider>
            <v-spacer></v-spacer>
            <v-dialog v-model="dialog" max-width="500px">
              <v-btn slot="activator" color="primary" dark class="mb-2">Novi trošak</v-btn>
              <v-card>
                <v-card-title>
                  <span class="headline">{{ formTitle }}</span>
                </v-card-title>

                <v-card-text>
                  <v-container grid-list-md>
                    <v-layout wrap>
                      <v-flex xs12 sm6 md4>
                        <v-text-field v-model="editedItem.rbr" label="Rbr."></v-text-field>
                      </v-flex>
                      <v-flex xs12 sm6 md4>
                        <v-text-field v-model="editedItem.nazivTroska" label="Naziv troška"></v-text-field>
                      </v-flex>
                      <v-flex xs12 sm6 md4>
                        <v-text-field v-model="editedItem.brojDokumenta" label="Broj dokumenta"></v-text-field>
                      </v-flex>
                      <v-flex xs12 sm6 md4>
                        <v-text-field v-model="editedItem.dobavljac" label="Dobavljač"></v-text-field>
                      </v-flex>
                      <v-flex xs12 sm6 md4>
                        <v-text-field class="inputPrice" type="number" v-model="editedItem.iznos" label="Iznos bez PDV-a"></v-text-field>
                      </v-flex>
                      <v-flex xs12 sm6 md4>
                        <v-text-field class="inputPrice" type="number" v-model="editedItem.pdv" label="PDV"></v-text-field>
                      </v-flex>
                    </v-layout>
                  </v-container>
                </v-card-text>

                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn color="blue darken-1" flat @click="close">Odustani</v-btn>
                  <v-btn color="blue darken-1" flat @click="save">Spremi</v-btn>
                </v-card-actions>
              </v-card>
            </v-dialog>
          </v-toolbar>
          <v-data-table :headers="headers" :items="troskovi" class="elevation-1">
            <template slot="items" slot-scope="props">
              <td class="text-xs-left" >{{ props.item.rbr }}</td>
              <td class="text-xs-left">{{ props.item.nazivTroska }}</td>
              <td class="text-xs-left" >{{ props.item.dobavljac }}</td>
              <td class="text-xs-left" >{{ props.item.brojDokumenta }}</td>
              <td class="text-xs-right">{{ formatMoney(props.item.iznos) }}</td>
              <td class="text-xs-right">{{ formatMoney(props.item.pdv) }}</td>
              <td class="text-xs-right">{{ formatMoney(props.item.ukupno) }}</td>
              <td class="justify-center layout px-0">
                <v-icon small class="mr-2" @click="editItem(props.item)">edit</v-icon>
                <v-icon small @click="deleteItem(props.item)">delete</v-icon>
              </td>
            </template>
            <template slot="no-data">
              <span>Unesite podatke</span>
            </template>
          </v-data-table>
        </div>
        <v-text-field
          v-model="ukupnoSUM"
          label="Ukupno (G)"
          class="text_field ukupno_g_text"
          readonly="true"
        ></v-text-field>
      </v-form>
      <div class="text-md-center text-lg-center text-xs-center">
        <v-pagination v-model="page" :length="15" :total-visible="5" class="pagi"></v-pagination>
      </div>
    </v-card>
  </v-container>
</template>

<script>
export default {
  data: () => ({
    valid: true,
    name: "",
    nameRules: [
      v => !!v || "Ime je obavezno",
      v => (v && v.length <= 255) || "Ime mora biti manje od 10 karaktera"
    ],
    email: "",
    emailRules: [
      v => !!v || "E-mail je obavezan",
      v => /.+@.+/.test(v) || "E-mail mora biti u ispravnom formatu"
    ],
    select: null,
    county: null,
    counties: [
      "Grad Zagreb",
      "Splitsko-dalmatinska",
      "Primorsko-goranska",
      "Dubrovačko-neretvanska"
    ],
    tipKorisnika: -1,
    pravniOblikKorisnika: -1,
    checkbox: false,
    page: 1,
    dialog: false,
    headers: [
        { text: 'Rbr. (A)', value: 'rbr' },
        {
          text: 'Naziv i opis troška (B)',
          align: 'left',
          sortable: false,
          value: 'nazivTroska'
        },
        { text: 'Naziv izvođača radova /dobavljača/ pružatelja usluge (C)', value: 'dobavljac' },
        { text: 'Broj ponude /predračuna /računa (D)', value: 'brojDokumenta' },
        { text: 'Iznos bez PDV-a (E)', value: 'iznos' },
        { text: 'Iznos PDV-a (F)', value: 'pdv' },
        { text: 'Ukupan iznos (F)', value: 'ukupno' }
      ],
    troskovi: [],
    editedIndex: -1,
    editedItem: {
      rbr: 0,
      nazivTroska: '',
      dobavljac: '',
      brojDokumenta: '',
      iznos: 0,
      pdv:0 ,
      ukupno: 0
    },
    defaultItem: {
      rbr: 0,
      nazivTroska: '',
      dobavljac: '',
      brojDokumenta: '',
      iznos: 0,
      pdv:0 ,
      ukupno: 0},
    headersPlovilo: [
        { text: 'Rbr. (A)', value: 'rbr' },
        { text: 'Ime i prezime', value: 'imePrezime' },
        { text: 'OIB', value: 'oib' }
      ],
    plovilo: [],
    editedIndexPlovilo: -1,
    editedItemPlovilo: {
      rbr: 0,
      imePrezime: '',
      oib: ''
    },
    defaultItemPlovilo: {
      rbr: 0,
      imePrezime: '',
      oib: ''
    },
  }),
    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'Novi trošak' : 'Ažuriraj trošak';
      },
      formTitlePlovilo () {
        return this.editedIndexPlovilo === -1 ? 'Novi suvlasnik' : 'Ažuriraj suvlasnika';
      },
      ukupnoSUM (){
        let sum = 0;
        this.troskovi.forEach(function(item) {
          sum += (parseFloat(item.ukupno));
        });

        return this.formatMoney(sum);
      },
    },
    watch: {
      dialog (val) {
        val || this.close()
      }
    },

    created () {
      this.initialize()
    },

    methods: {
      initialize () {
        this.troskovi = [

        ]
      },
      formatMoney(amount, decimalCount = 2, decimal = ",", thousands = ".") {
        decimalCount = Math.abs(decimalCount);
        decimalCount = isNaN(decimalCount) ? 2 : decimalCount;

        const negativeSign = amount < 0 ? "-" : "";

        let i = parseInt(amount = Math.abs(Number(amount) || 0).toFixed(decimalCount)).toString();
        let j = (i.length > 3) ? i.length % 3 : 0;

        return negativeSign + (j ? i.substr(0, j) + thousands : '') + i.substr(j).replace(/(\d{3})(?=\d)/g, "1" + thousands) + (decimalCount ? decimal + Math.abs(amount - i).toFixed(decimalCount).slice(2) : "")+" kn";
      },
      editItem (item) {
        this.editedIndex = this.troskovi.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },

      deleteItem (item) {
        const index = this.troskovi.indexOf(item)
        confirm('Jeste li sigurni da želite obrisati stavku?') && this.troskovi.splice(index, 1)
      },

      close () {
        this.dialog = false
        setTimeout(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        }, 300)
      },

      save () {
        this.editedItem.ukupno=(Number(this.editedItem.iznos)+Number(this.editedItem.pdv)).toFixed(2);
        if (this.editedIndex > -1) {
          Object.assign(this.troskovi[this.editedIndex], this.editedItem)
        } else {
          this.troskovi.push(this.editedItem)
        }
        this.close()
      },

      editItemPlovilo (item) {
        this.editedIndexPlovilo = this.plovilo.indexOf(item)
        this.editedItemPlovilo = Object.assign({}, item)
        this.dialog = true
      },

      deleteItemPlovilo (item) {
        const index = this.plovilo.indexOf(item)
        confirm('Jeste li sigurni da želite obrisati stavku?') && this.plovilo.splice(index, 1)
      },

      closePlovilo () {
        this.dialog = false
        setTimeout(() => {
          this.editedItemPlovilo = Object.assign({}, this.defaultItemPlovilo)
          this.editedIndexPlovilo = -1
        }, 300)
      },

      savePlovilo () {
        
        if (this.editedIndexPlovilo > -1) {
          Object.assign(this.plovilo[this.editedIndexPlovilo], this.editedItemPlovilo)
        } else {
          this.plovilo.push(this.editedItemPlovilo)
        }
        this.close()
      },

    submit() {
      if (this.$refs.form.validate()) {
        // Form is valid, process
      }
    },
    clear() {
      this.$refs.form.reset();
    }
  }
};
</script>

<style>

.inputPrice input[type='number'] {
    -moz-appearance:textfield;
}
.inputPrice input::-webkit-outer-spin-button,
.inputPrice input::-webkit-inner-spin-button {
    -webkit-appearance: none;
}


:root {
  --primary-color: #0868ac !important;
  --secondary-color: #fff !important;
  --third-color: rgba(0,0,0,0.87)!important;
  --message-color: #FF4136 !important;
}

/* Theming */
h1,
.v-input--radio-group__input .v-label:first-child {
  color: var(--primary-color);
}

.theme--light.v-table thead tr:first-child {
  border-bottom: 3px solid var(--primary-color);
}

.theme--light.v-label {
  color: var(--primary-color) !important;
}

.theme--light.v-card {
  color: var(--primary-color) !important;
}

.theme--light.v-messages {
  color: var(--message-color) !important;
}

.theme--light.v-datatable .v-datatable__actions {
  margin-bottom: 15px;
}

/*----- custom ------------- */

.v_card {
  position: relative;
  width: 100%;
  max-width: 1050px;
  margin: 0;
  padding: 5%;
  margin: 0 auto;
}

.text_field {
  width: 100%;
  max-width: 450px;
}

.text_field:nth-of-type(1) {
  margin-top: 40px;
}

.clean-btn:hover {
  background: var(--primary-color) !important;
  color: var(--secondary-color);
  box-shadow: 3px solid #043e67;
}

.theme--light.v-pagination .v-pagination__item--active,
.theme--dark.v-btn:not(.v-btn--icon):not(.v-btn--flat) {
  background: var(--primary-color) !important;
   color: var(--secondary-color);
}

.v-toolbar__content { 
    margin-top: 50px !important;
}

.v-input.text_field.v-text-field.v-input--is-label-active.v-input--is-dirty.v-input--is-readonly.theme--light {
  margin-top: 30px;
}

/* !!! */
.v-dialog:not(.v-dialog--fullscreen) {
  max-height: none;
}

th.column.text-xs-left {
  color: var(--primary-color) !important;
  font-weight: bold;
  font-size: 16px;
}

.headline {
  color: var(--primary-color) !important;
  font-weight: bold;
  text-align: center;
  margin: 10px auto 0;
}

.ukupno_g_text {
  width: 100px;
}
.theme--light.v-datatable thead th.column.sortable .v-icon,
.theme--light.v-datatable thead th.column.sortable.active .v-icon {
  color: var(--primary-color) !important;
}

/* responsive */

@media only screen and (max-width: 320px) {
  .pagi {
    position: absolute;
    left: -7px;
    bottom: 0;
  }

  .clean-btn {
    margin-bottom: 50px !important;
  }
}

@media only screen and (max-width: 600px) {
  h1 {
    font-size: 22px;
  }

  h2 {
    font-size: 18px;
    margin-top: 10px;
  }

  .clean-btn {
    display: block;
    margin: 20px auto 20px;
  }

  .v-toolbar__title {
    width: 200px;
  }

  .reset-btn {
    position: absolute;
    bottom: 0
  }
}
</style>

<template>
  <v-stepper v-model="e1">
    <v-stepper-header>
      <v-stepper-step :complete="e1 > 1" step="1"> Infomation </v-stepper-step>
      <v-divider></v-divider>
      <v-stepper-step :complete="e1 > 2" step="2"> Payment </v-stepper-step>
      <v-divider></v-divider>
    </v-stepper-header>

    <v-stepper-items>
      <v-stepper-content step="1">
        <v-container>
          <div class="row justify-center">
            <v-col cols="12" sm="6" md="4" class="mr-5">
              <div class="text-h6 mb-4">Contact Infomation</div>
              <v-divider class="mb-4"></v-divider>
              <v-row>
                <v-col>
                  <v-text-field
                    :rules="[rules.required]"
                    label="Email"
                    outlined
                    dense
                  ></v-text-field>
                </v-col>
              </v-row>
              <div class="text-h6 mb-4">Billing Address</div>
              <v-divider class="mb-4"></v-divider>
              <v-form ref="form" v-model="valid">
                <v-row>
                  <v-col>
                    <v-text-field
                      dense
                      :rules="[rules.required]"
                      label="First name"
                      outlined
                    ></v-text-field>
                  </v-col>
                  <v-col>
                    <v-text-field
                      :rules="[rules.required]"
                      label="Last name"
                      outlined
                      dense
                    ></v-text-field>
                  </v-col>
                </v-row>
                <v-text-field
                  label="Company(Optional)"
                  outlined
                  dense
                ></v-text-field>
                <v-text-field
                  :rules="[rules.required]"
                  label="Address"
                  outlined
                  dense
                ></v-text-field>
                <v-text-field
                  label="Apartment,suite,etc... "
                  outlined
                  dense
                ></v-text-field>
                <v-text-field
                  :rules="[rules.required]"
                  label="City"
                  outlined
                  dense
                ></v-text-field>
                <v-row>
                  <v-col>
                    <v-select
                      :items="items"
                      label="Country/Region"
                      outlined
                      dense
                    ></v-select>
                  </v-col>
                  <v-col>
                    <v-text-field
                      :rules="[rules.required]"
                      label="Postal Code"
                      outlined
                      dense
                    ></v-text-field>
                  </v-col>
                </v-row>

                <v-text-field
                  :rules="[rules.required]"
                  label="Phone"
                  outlined
                  dense
                ></v-text-field>
              </v-form>
              <v-btn color="primary" :disabled="!valid" large @click="e1 = 2">
                Continue
              </v-btn>
            </v-col>

            <v-col cols="12" md="6" sm="4" class="ml-5">
              <div class="text-h6 mb-4">Items in cart</div>
              <v-divider class="mb-4"></v-divider>
              <v-row class="ml-5" justify="center">
                <v-list-item
                  v-for="product in cart"
                  :key="product.id"
                  class="my-3"
                >
                  <v-badge overlap color="pink">
                    <span slot="badge"> {{ product.quantity }}</span>
                    <v-avatar class="pt-3" rounded width="70" height="60">
                      <v-img
                        :src="product.product.img"
                        width="70"
                        height="60"
                      ></v-img>
                    </v-avatar>
                  </v-badge>

                  <v-list-item two-line>
                    <v-list-item-content>
                      <div class="min-w-0 flex-grow-1">
                        <v-list-item-title
                          class="mb-1 font-weight-bold truncate"
                        >
                          {{ product.product.title }}</v-list-item-title
                        >
                        <v-list-item-subtitle
                          v-if="product.product.price == 0"
                          class="
                            red--text
                            text--darken-4
                            font-weight-black
                            mb-1
                          "
                          >Free</v-list-item-subtitle
                        >
                        <v-list-item-subtitle
                          v-else
                          class="
                            red--text
                            text--darken-4
                            font-weight-black
                            mb-1
                          "
                          >${{
                            parseFloat(product.product.price).toFixed(2)
                          }}</v-list-item-subtitle
                        >
                      </div>
                    </v-list-item-content>
                  </v-list-item>
                </v-list-item>
              </v-row>
              <v-divider class="my-4" v-if="cart.length > 0"></v-divider>
              <v-row justify="center" v-if="cart.length > 0">
                 <v-spacer></v-spacer>
                <div
                  class="text-h5 font-weight-bold"    
                >Total: </div>
               
                <div
                  class="text-h5 font-weight-bold" style="color: #eb3452"  
                >{{totalPrice}}$</div>
              </v-row>
            </v-col>
          </div>

          <v-divider vertical dark></v-divider>
        </v-container>
      </v-stepper-content>

      <v-stepper-content step="2">
        <v-container>
          <div class="row justify-center">
            <v-col  cols="12" sm="6" md="4">
              <v-simple-table style="border-style: solid; border-width: 1px ">
                <tbody>
                  <tr>
                    <td>Contact</td>
                    <td>example@gmail.com</td>
                  </tr>
                  <tr>
                    <td>Billing</td>
                    <td>example, example, VietNam</td>
                  </tr>
                </tbody>
              </v-simple-table>
               <div class="text-h6 mt-4">Add tip</div>
              <v-divider class="mb-4"></v-divider>
                <v-text-field outlined label="Support our team" type="number"></v-text-field>
               <div class="text-h6 mt-4">Payment</div>
              <v-divider class="mb-4"></v-divider>
              <v-form ref="form" v-model="valid" class="my-6">
                <v-radio-group row>
                  <v-radio label="MasterCard">
                    
                  </v-radio>
                  <v-radio label="Visa">
                    
                  </v-radio>
                  <v-radio label="Napas">
                   
                  </v-radio>
                </v-radio-group>
                <v-text-field
                  label="Card number"
                  outlined
                   :rules="[rules.required]"
                ></v-text-field>
                <v-text-field
                  :rules="[rules.required]"
                  label="Name on card"
                  outlined           
                ></v-text-field>
                <v-row>
                  <v-col>
                    <v-text-field
                      :rules="[rules.required]"
                      label="Expiration date (MM/YY)"
                      outlined
                    
                    ></v-text-field>
                  </v-col>
                  <v-col>
                    <v-text-field
                      :rules="[rules.required]"
                      label="Security code"
                      outlined
                    
                    ></v-text-field>
                  </v-col>
                </v-row>

              </v-form>
               <v-btn color="primary"> Complete Payment </v-btn>

        <v-btn text @click="e1 = 1"> Return </v-btn>
            </v-col>
          </div>
        </v-container>
       
      </v-stepper-content>
    </v-stepper-items>
  </v-stepper>
</template>
<script>
import { mapGetters } from "vuex";
export default {
  data() {
    return {
      name: "Checkout",
      items: ["United States", "Canada", "Afghanistan", "Albania", "Algeria", "American Samoa", "Andorra", "Angola", "Anguilla", 
      "Antarctica", "Antigua and/or Barbuda", "Argentina", "Armenia", "Aruba", "Australia", "Austria", "Azerbaijan", "Bahamas", "Bahrain", 
      "Bangladesh", "Barbados", "Belarus", "Belgium", "Belize", "Benin", "Bermuda", "Bhutan", "Bolivia", "Bosnia and Herzegovina", "Botswana", "Bouvet Island", 
      "Brazil", "British Indian Ocean Territory", "Brunei Darussalam", "Bulgaria", "Burkina Faso", "Burundi", "Cambodia", "Cameroon", "Cape Verde", "Cayman Islands", 
      "Central African Republic", "Chad", "Chile", "China", "Christmas Island", "Cocos (Keeling) Islands", "Colombia", "Comoros", "Congo", "Cook Islands", "Costa Rica", 
      "Croatia (Hrvatska)", "Cuba", "Cyprus", "Czech Republic", "Denmark", "Djibouti", "Dominica", "Dominican Republic", "East Timor", "Ecudaor", "Egypt", "El Salvador", 
      "Equatorial Guinea", "Eritrea", "Estonia", "Ethiopia", "Falkland Islands (Malvinas)", "Faroe Islands", "Fiji", "Finland", "France", "France, Metropolitan",
       "French Guiana", "French Polynesia", "French Southern Territories", "Gabon", "Gambia", "Georgia", "Germany", "Ghana", "Gibraltar", "Greece", "Greenland",
        "Grenada", "Guadeloupe", "Guam", "Guatemala", "Guinea", "Guinea-Bissau", "Guyana", "Haiti", "Heard and Mc Donald Islands", "Honduras", "Hong Kong", 
        "Hungary", "Iceland", "India", "Indonesia", "Iran (Islamic Republic of)", "Iraq", "Ireland", "Israel", "Italy", "Ivory Coast", "Jamaica", "Japan", "Jordan", 
        "Kazakhstan", "Kenya", "Kiribati", "Korea, Democratic People's Republic of", "Korea, Republic of", "Kosovo", "Kuwait", "Kyrgyzstan", "Lao People's Democratic Republic",
        "Latvia", "Lebanon", "Lesotho", "Liberia", "Libyan Arab Jamahiriya", "Liechtenstein", "Lithuania", "Luxembourg", "Macau", "Macedonia", "Madagascar", "Malawi", "Malaysia",
         "Maldives", "Mali", "Malta", "Marshall Islands", "Martinique", "Mauritania", "Mauritius", "Mayotte", "Mexico", "Micronesia, Federated States of", "Moldova, Republic of",
        "Monaco", "Mongolia", "Montserrat", "Morocco", "Mozambique", "Myanmar", "Namibia", "Nauru", "Nepal", "Netherlands", "Netherlands Antilles", "New Caledonia", "New Zealand",
        "Nicaragua", "Niger", "Nigeria", "Niue", "Norfork Island", "Northern Mariana Islands", "Norway", "Oman", "Pakistan", "Palau", "Panama", "Papua New Guinea", "Paraguay",
        "Peru", "Philippines", "Pitcairn", "Poland", "Portugal", "Puerto Rico", "Qatar", "Reunion", "Romania", "Russian Federation", "Rwanda", "Saint Kitts and Nevis",
        "Saint Lucia", "Saint Vincent and the Grenadines", "Samoa", "San Marino", "Sao Tome and Principe", "Saudi Arabia", "Senegal", "Seychelles", "Sierra Leone", "Singapore", 
        "Slovakia", "Slovenia", "Solomon Islands", "Somalia", "South Africa", "South Georgia South Sandwich Islands", "South Sudan", "Spain", "Sri Lanka", "St. Helena", 
        "St. Pierre and Miquelon", "Sudan", "Suriname", "Svalbarn and Jan Mayen Islands", "Swaziland", "Sweden", "Switzerland", "Syrian Arab Republic", "Taiwan", "Tajikistan", 
        "Tanzania, United Republic of", "Thailand", "Togo", "Tokelau", "Tonga", "Trinidad and Tobago", "Tunisia", "Turkey", "Turkmenistan", "Turks and Caicos Islands", "Tuvalu", 
        "Uganda", "Ukraine", "United Arab Emirates", "United Kingdom", "United States minor outlying islands", "Uruguay",
       "Uzbekistan", "Vanuatu", "Vatican City State", "Venezuela", "Vietnam", "Virigan Islands (British)", "Virgin Islands (U.S.)", "Wallis and Futuna Islands",
        "Western Sahara", "Yemen", "Yugoslavia", "Zaire", "Zambia", "Zimbabwe"],
      e1: 1,
      valid: true,
      rules: {
        required: (value) => !!value || "Required.",
      },
    };
  },
  computed: {
    ...mapGetters(["cart", "totalPrice"]),
  },
  methods: {
    validate() {
      this.$refs.form.validate();
    },
  },
};
</script>
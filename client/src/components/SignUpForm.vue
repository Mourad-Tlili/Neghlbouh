<template>
  <div>
    <v-row>
      <v-col class="" cols="12" sm="6">
        <v-form
          :class="$vuetify.breakpoint.xs ? '' : 'form '"
          ref="form"
          v-model="valid"
        >
          <v-row align="center">
            <v-col
              :class="$vuetify.breakpoint.xs ? 'mt-5' : 'mt-7'"
              align="center"
              cols="12"
            >
              <span class="text-center  font-login-popup2">
                {{ text.popup_text2 }}
              </span>
              <v-alert
                class="mt-10"
                dense
                outlined
                type="error"
                v-if="error !== ''"
                width="80%"
              >
                {{ error.message }}
              </v-alert>
            </v-col>
          </v-row>
          <v-card-text>
            <v-form ref="form" v-model="valid">
              <!-- Name -->
              <v-row class="mb-0 pb-0" justify="center">
                <v-col class="mb-0 pb-0" cols="10">
                  <v-text-field
                    :label="text.nameField"
                    :rules="nameRules"
                    class="mb-0 pb-0"
                    clearable
                    color="black"
                    filled
                    outlined
                    prepend-inner-icon="fas fa-user"
                    required
                    rounded
                    v-model="name"
                  ></v-text-field>
                </v-col>
              </v-row>
              <!-- City  -->
              <v-row class="mb-0 pb-0" justify="center">
                <v-col class="mb-0 pb-0" cols="10">
                  <v-select
                    :items="cities"
                    :label="text.cityField"
                    class="mb-0 pb-0"
                    clearable
                    color="black"
                    filled
                    outlined
                    prepend-inner-icon="fas fa-thumbtack"
                    required
                    rounded
                    v-model="city"
                  ></v-select>
                </v-col>
              </v-row>
              <!-- Area  -->
              <v-row class="mb-0 pb-0" justify="center">
                <v-col class="mb-0 pb-0" cols="10">
                  <v-select
                    :items="areaCal"
                    :label="text.areaField"
                    class="mb-0 pb-0"
                    clearable
                    color="black"
                    filled
                    outlined
                    prepend-inner-icon="fas fa-thumbtack"
                    required
                    rounded
                    v-model="area"
                  ></v-select>
                </v-col>
              </v-row>
              <!-- Phone  -->
              <v-row class="mb-0 pb-0" justify="center">
                <v-col class="mb-0 pb-0" cols="10">
                  <v-text-field
                    :label="text.phoneField"
                    :rules="phoneRules"
                    class="mb-0 pb-0"
                    clearable
                    color="black"
                    outlined
                    prepend-inner-icon="fas fa-phone"
                    required
                    rounded
                    type="integer"
                    v-model="phone"
                  ></v-text-field>
                </v-col>
              </v-row>
              <!-- Email  -->
              <v-row class="mb-0 pb-0" justify="center">
                <v-col class="mb-0 pb-0" cols="10">
                  <v-text-field
                    :label="text.emailField"
                    :rules="emailRules"
                    class="mb-0 pb-0"
                    clearable
                    color="black"
                    filledv
                    outlined
                    prepend-inner-icon="far fa-envelope"
                    rounded
                    v-model="email"
                  ></v-text-field>
                </v-col>
              </v-row>
              <!-- CIN  -->
              <v-row class="mb-0 pb-0" justify="center">
                <v-col class="mb-0 pb-0" cols="10">
                  <v-text-field
                    :label="text.cinField"
                    :rules="cinRules"
                    class="mb-0 pb-0"
                    clearable
                    color="black"
                    filled
                    outlined
                    prepend-inner-icon="far fa-address-card"
                    required
                    rounded
                    v-model="CIN"
                  ></v-text-field>
                </v-col>
              </v-row>
              <!-- Password  -->
              <v-row class="mb-0 pb-0" justify="center">
                <v-col cols="10">
                  <v-text-field
                    :append-icon="show1 ? 'fas fa-eye' : 'fas fa-eye-slash'"
                    :label="text.passWordField"
                    :type="show1 ? 'text' : 'password'"
                    @click:append="show1 = !show1"
                    class="mt-0 pb-0"
                    clearable
                    color="black"
                    filled
                    outlined
                    prepend-inner-icon="fas fa-lock"
                    required
                    rounded
                    v-model="password"
                  >
                  </v-text-field>
                </v-col>
              </v-row>
            </v-form>
            <!-- submit Button  -->
            <v-row justify="center">
              <v-col align="center" cols="10">
                <v-hover v-slot:default="{ hover }">
                  <!-- when the button gets hovered hover becomes true so we switch the class -->
                  <v-btn
                    :class="hover ? 'mt--10 glowing-border' : 'mt--10'"
                    :disabled="!valid"
                    :loading="loading"
                    :ripple="{ class: 'red--text' }"
                    @click="validate"
                    color="#df0100"
                    height="225%"
                    rounded
                    width="100%"
                  >
                    <span class="font-login-white">
                      {{ text.submitField }}
                    </span>
                  </v-btn>
                </v-hover>
              </v-col>
            </v-row>
          </v-card-text>
        </v-form>
      </v-col>
      <v-col :class="$vuetify.breakpoint.xs ? 'd-none' : ''" sm="6">
        <v-img
          class="d-flex align-end pb-12 mt-12 side-image"
          src="@/assets/SignUpPic.png"
        >
          <v-row>
            <v-col class="text-center" cols="12">
              <span class="text-photo">{{ text.text_photo }}</span>
            </v-col>
          </v-row>
        </v-img>
      </v-col>
    </v-row>
  </div>
</template>

<script>
import { mapActions, mapState } from "vuex";
import authController from "@/services/AuthenticationService";
import data from "../store/data.json";

export default {
  name: "SignUpForm",
  props: {
    isModal: {
      Boolean,
      default: false
    }
  },
  data() {
    return {
      loading: false,
      error: "",
      password: "",
      CIN: "",
      cinRules: [
        v => !!v || "رقم بطاقة التعريف مطلوب",
        v => (!isNaN(parseFloat(v)) && !isNaN(v - 0)) || "يجب أن يكون رقم",
        v => (v && v.length <= 8) || "يجب أن يتكون من 8 أرقام"
      ],
      name: "",
      nameRules: [
        v => !!v || "الاسم مطلوب",
        v => (v && v.length <= 35) || "يجب أن يكون أقل من 35 حرف"
      ],
      city: "",
      area: "",
      phone: "",
      phoneRules: [
        v => !!v || "رقم الهاتف مطلوب",
        v => (!isNaN(parseFloat(v)) && !isNaN(v - 0)) || "يجب أن يكون رقم",
        v => (v && v.length == 8) || "يجب أن يتكون من 8 أرقام"
      ],
      email: "",
      emailRules: [
        v => !!v || "البريد الالكتروني مطلوب",
        v => /.+@.+\..+/.test(v) || "يجب ان يكون البريد الاكتروني صحيح"
      ],
      valid: false,
      show1: false,
      cities: data.cities,
      areas: []
    };
  },
  computed: {
    ...mapState(["langPack"]),
    areaCal() {
      if (!this.city) {
        return data.allAreas;
      } else {
        return data.areas[this.city];
      }
    },
    text() {
      return this.langPack.Sign_Up;
    }
  },
  methods: {
    ...mapActions(["pressLogin"]),
    async validate() {
      this.$refs.form.validate();
      if (this.valid) {
        this.loading = true;
        try {
          await authController.register({
            name: this.name,
            cin: this.CIN,
            email: this.email,
            city: this.city,
            phone: this.phone,
            password: this.password,
            area: this.area
          });
          this.loading = false;
          this.$router.replace({ name: "SignIn" });
        } catch (e) {
          this.loading = false;
          this.error = e.response.data.err;
        }
      }
    }
  }
};
</script>

<style scoped>
.form {
  /* This form has been down-scaled and the z-index is made 1
           with position relative so it doesn't collide with the nav bar */
  position: relative;
  z-index: 1 !important;
}

body * {
  font-family: Cairo;
}

.mt--5 {
  margin-top: -25px;
}

.mt--10 {
  margin-top: -50px !important;
}

.font-login {
  font-size: 2rem !important;
}

.font-login-popup2 {
  font-size: 1.4rem !important;
  background-color: rgba(255, 255, 255, 0);
  color: #616161;
}

.font-login-white {
  font-size: 1.5rem !important;
  background-color: rgba(255, 255, 255, 0);
  color: #ffffff;
}

.font-login-pass {
  font-size: 1rem !important;
  background-color: rgba(255, 255, 255, 0);
  color: #c70100;
}

.glowing-border {
  box-shadow: 0px 0 10px 1px #df0100, /* inner white */ 0px 0 10px 1px #770000,
    /* middle magenta */ 0 0 10px 1px #6b2028 !important;
}

.side-image {
  height: 90vh;
  width: 70vw;
  padding-bottom: 120px !important;
}

.text-photo {
  font-size: 45px;
  color: white;
}
</style>

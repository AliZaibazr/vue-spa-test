<template>
  <div>
    <b-form @submit.prevent="onSubmit" @reset="onReset" v-if="show" enctype=multipart/form-data>
      <b-form-group id="name" label="Your Name:" label-for="name">
        <b-form-input
          id="name"
          v-model="$v.form.name.$model"
          placeholder="Enter name"
          required
        ></b-form-input>
        <b-form-invalid-feedback
          :state="!$v.form.name.$anyError"
          v-if="showError"
        >
          Your name is required
        </b-form-invalid-feedback>
      </b-form-group>
      <!-- <b-form-valid-feedback :state="!$v.form.name.$anyError">
        Looks Good.
      </b-form-valid-feedback> -->
      <b-form-group id="email" label="Email address:" label-for="email">
        <b-form-input
          id="email"
          v-model="$v.form.email.$model"
          type="email"
          placeholder="Enter email"
          required
          email
        ></b-form-input>
        <b-form-invalid-feedback
          :state="!$v.form.email.$anyError"
          v-if="showError"
        >
          <div v-if="!$v.form.email.required">Email is required</div>
          <div v-if="!$v.form.email.email">Email format is not correct</div>
        </b-form-invalid-feedback>
      </b-form-group>
      <b-form-group
        id="phone-number"
        label="Phone Number:"
        label-for="phone-number"
      >
        <b-form-input
          id="phone-number"
          v-model="$v.form.phone.$model"
          type="text"
          placeholder="Enter Phone Number  (123) 4567899."
          required
        ></b-form-input>
        <b-form-invalid-feedback
          :state="!$v.form.phone.$anyError"
          v-if="showError"
        >
          <div v-if="!$v.form.phone.required">Phone is required</div>
          <div v-if="!$v.form.phone.phone_formate">
            Phone format must be (123) 456-7899
          </div>
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group>
        <b-form-textarea
          id="address"
          v-model="$v.form.address.$model"
          placeholder="Enter Address..."
          rows="3"
          max-rows="6"
          required
        ></b-form-textarea>
        <b-form-invalid-feedback
          :state="!$v.form.address.$anyError"
          v-if="showError"
        >
          <div v-if="!$v.form.address.required">Address is required</div>
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group
        id="zip-code"
        label="Zip Code:"
        label-for="zip-code"
        description="Enter Five Digit Zip Code."
      >
        <b-form-input
          id="zip-code"
          v-model="$v.form.zip.$model"
          :validated="false"
          type="text"
          placeholder="Enter Zip Code"
          required
        ></b-form-input>
        <b-form-invalid-feedback
          :state="!$v.form.zip.$anyError"
          v-if="showError"
        >
          <div v-if="!$v.form.zip.required">Zip code is required</div>
          <div v-if="!$v.form.zip.zip_formate">Zip format must be 5 digit</div>
        </b-form-invalid-feedback>
      </b-form-group>

      <b-form-group>
        <b-form-file
          v-model="$v.form.profile_img.$model"
          :state="
            showError == false ? null : Boolean($v.form.profile_img.$model)
          "
          placeholder="Choose a Profile Image or drop it here..."
          drop-placeholder="Drop file here..."
          required
        ></b-form-file>
        <b-form-invalid-feedback
          :state="!$v.form.profile_img.$anyError"
          v-if="showError"
        >
          <div v-if="!$v.form.profile_img.required">
            Profile Image is required
          </div>
          <div v-if="!$v.form.profile_img.profile_img_ext">
            Only jpeg, jpg or png accepted
          </div>
        </b-form-invalid-feedback>
      </b-form-group>
      <b-form-group>
        <b-form-file
          class="mt-3 mb-3"
          v-model="$v.form.license_img.$model"
          :state="
            showError == false ? null : Boolean($v.form.license_img.$model)
          "
          placeholder="Choose a License or drop it here..."
          drop-placeholder="Drop file here..."
          required
        ></b-form-file>
        <b-form-invalid-feedback
          :state="!$v.form.license_img.$anyError"
          v-if="showError"
        >
          <div v-if="!$v.form.license_img.required">License is required</div>
          <div v-if="!$v.form.license_img.license_img_ext">
            Only jpeg, jpg, png, doc, pdf or docx accepted
          </div>
        </b-form-invalid-feedback>
      </b-form-group>
      <b-button type="button" variant="primary mr-2" @click="onSubmit"
        >Submit
      </b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
    <b-card class="mt-3" header="Form Data Result">
      <pre class="m-0">{{ form }}</pre>
      <span>Validation</span>
      <pre class="m-0">{{ $v.form.name }}</pre>
    </b-card>
  </div>
</template>

<script>
const axios = require("axios");
import { required, email } from "vuelidate/lib/validators";
import { helpers } from "vuelidate/lib/validators";
const phone_formate = helpers.regex("phone_formate", /^\d{3}\d{3}\d{4}$/);
const zip_formate = helpers.regex("zip_formate", /^\d{5}$/);
const profile_img_ext = helpers.regex(
  "profile_img_ext",
  /^.*\.(jpg|JPG|png|PNG|jpeg|JPEG)$/
);
const license_img_ext = helpers.regex(
  "license_img_ext",
  /^.*\.(jpg|JPG|png|PNG|jpeg|JPEG|pdf|PDF|doc|DOC|docx|docx)$/
);
export default {
  data() {
    return {
      form: {
        name: "",
        email: "",
        phone: "",
        address: "",
        zip: "",
        profile_img: null,
        license_img: null
      },
      showError: false,
      show: true
    };
  },
  validations: {
    form: {
      name: {
        required
      },
      email: {
        required,
        email
      },
      phone: {
        required,
        phone_formate
      },
      address: {
        required
      },
      zip: {
        required,
        zip_formate
      },
      profile_img: {
        required,
        name: {
          profile_img_ext
        }
      },
      license_img: {
        required,
        name: {
          license_img_ext
        }
      }
    }
  },
  methods: {
    onSubmit() {
      this.showError = true;
      if(!this.$v.form.$anyError)
      {
          var formData = new FormData();
          formData.append("name", this.form.name);
          formData.append("email", this.form.email);
          formData.append("address", this.form.address);
          formData.append("phone", this.form.phone);
          formData.append("zip", this.form.zip);
          formData.append("license_img_file", this.form.license_img);
          formData.append("profile_img_file", this.form.profile_img);

          axios.post('http://127.0.0.1:8000/api/info', formData)
        .then(function (response) {
            console.log(response);
        })
        .catch(function (error) {
            console.log(error);
        });
      }
    },
    onReset(event) {
      event.preventDefault();
      //   // Reset our form values
      this.form.email = "";
      //   this.form.name = "";
      //   this.form.food = null;
      //   this.form.checked = [];
      //   // Trick to reset/clear native browser form validation state
      //   this.show = false;
      //   this.$nextTick(() => {
      //     this.show = true;
      //   });
    }
  }
};
</script>

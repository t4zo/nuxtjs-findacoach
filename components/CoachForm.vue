<template>
  <BaseCard>
    <form @submit.prevent="submitForm">
      <div class="form-control" :class="{ invalid: !firstName.isValid }">
        <label for="firstName">First Name</label>
        <input
          type="text"
          id="firstName"
          v-model.trim="firstName.value"
          @blur="clearValidity('firstName')"
        />
        <p v-if="!firstName.isValid">First Name must not be empty</p>
      </div>
      <div class="form-control" :class="{ invalid: !lastName.isValid }">
        <label for="lastName">Last Name</label>
        <input
          type="text"
          id="lastName"
          v-model.trim="lastName.value"
          @blur="clearValidity('lastName')"
        />
        <p v-if="!lastName.isValid">Last Name must not be empty</p>
      </div>
      <div class="form-control" :class="{ invalid: !description.isValid }">
        <label for="description">Description Name</label>
        <textarea
          type="text"
          id="description"
          rows="5"
          v-model.trim="description.value"
          @blur="clearValidity('description')"
        />
        <p v-if="!description.isValid">Description must not be empty</p>
      </div>
      <div class="form-control" :class="{ invalid: !rate.isValid }">
        <label for="rate">Hourly Rate</label>
        <input
          type="number"
          id="rate"
          v-model.number="rate.value"
          @blur="clearValidity('rate')"
        />
        <p v-if="!rate.isValid">Rate must be greater than 0</p>
      </div>
      <div class="form-control" :class="{ invalid: !areas.isValid }">
        <h3>Areas of Expertise</h3>
        <div>
          <input
            type="checkbox"
            name="frontend"
            id="frontend"
            value="frontend"
            v-model="areas.value"
            @blur="clearValidity('areas')"
          />
          <label for="frontend">Frontend Development</label>
        </div>
        <div>
          <input
            type="checkbox"
            name="backend"
            id="backend"
            value="backend"
            v-model="areas.value"
            @blur="clearValidity('areas')"
          />
          <label for="backend">Backend Development</label>
        </div>
        <div>
          <input
            type="checkbox"
            name="career"
            id="career"
            value="career"
            v-model="areas.value"
            @blur="clearValidity('areas')"
          />
          <label for="career">Career Advisory</label>
        </div>
        <p v-if="!areas.isValid">At least one expertise must be selected</p>
      </div>
      <p v-if="!formIsValid">Please fix the above errors and submit again.</p>
      <BaseButton>Register</BaseButton>
    </form>
  </BaseCard>
</template>

<script>
import BaseButton from "@/components/ui/BaseButton.vue";
import BaseCard from "@/components/ui/BaseCard.vue";

export default {
  components: { BaseButton, BaseCard },
  emits: ["form-submitted"],
  data() {
    return {
      firstName: {
        value: "",
        isValid: true,
      },
      lastName: {
        value: "",
        isValid: true,
      },
      description: {
        value: "",
        isValid: true,
      },
      rate: {
        value: null,
        isValid: true,
      },
      areas: {
        value: [],
        isValid: true,
      },
      formIsValid: true,
    };
  },
  methods: {
    clearValidity(input) {
      this[input].isValid = true;
    },
    validateForm() {
      this.formIsValid = true;
      if (this.firstName.value === "") {
        this.firstName.isValid = false;
        this.formIsValid = false;
      }
      if (this.lastName.value === "") {
        this.lastName.isValid = false;
        this.formIsValid = false;
      }
      if (this.description.value === "") {
        this.description.isValid = false;
        this.formIsValid = false;
      }
      if (!this.rate.value || this.rate.value < 0) {
        this.rate.isValid = false;
        this.formIsValid = false;
      }
      if (this.areas.value.length === 0) {
        this.areas.isValid = false;
        this.formIsValid = false;
      }
    },
    submitForm() {
      this.validateForm();

      if (!this.formIsValid) {
        return;
      }

      const formData = {
        first: this.firstName.value,
        last: this.lastName.value,
        desc: this.description.value,
        rate: this.rate.value,
        areas: this.areas.value,
      };

      this.$emit("form-submitted", formData);
    },
  },
};
</script>

<style scoped>
.form-control {
  margin: 0.5rem 0;
}

label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
}

input[type="checkbox"] + label {
  font-weight: normal;
  display: inline;
  margin: 0 0 0 0.5rem;
}

input,
textarea {
  display: block;
  width: 100%;
  border: 1px solid #ccc;
  font: inherit;
}

input:focus,
textarea:focus {
  background-color: #f0e6fd;
  outline: none;
  border-color: #3d008d;
}

input[type="checkbox"] {
  display: inline;
  width: auto;
  border: none;
}

input[type="checkbox"]:focus {
  outline: #3d008d solid 1px;
}

h3 {
  margin: 0.5rem 0;
  font-size: 1rem;
}

.invalid label {
  color: red;
}

.invalid input,
.invalid textarea {
  border: 1px solid red;
}
</style>
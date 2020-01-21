<template>
  <div class="home">
    <form class="form" @submit.prevent="onSubmit">
      <div class="input">
        <label for="email">Email</label>
        <input
          :class="{ error: $v.email.$error }"
          type="email"
          id="email"
          @blur="$v.email.$touch()"
          v-model.trim="email"
        />

        <div v-if="$v.email.$dirty">
          <p class="error-message" v-if="!$v.email.email">
            Please enter a valid email address.
          </p>
          <p class="error-message" v-if="!$v.email.required">
            Email must not be empty.
          </p>
        </div>
      </div>
      <div class="input">
        <label for="firstName">First Name</label>
        <input
          :class="{ error: $v.firstName.$error }"
          type="text"
          id="firstName"
          v-model.trim="firstName"
          @input="$v.firstName.$touch()"
        />

        <div v-if="$v.firstName.$dirty">
          <p class="error-message" v-if="!$v.firstName.required">
            First Name must not be empty.
          </p>
        </div>
      </div>
      <div class="input">
        <label for="lastName">Last Name</label>
        <input
          :class="{ error: $v.lastName.$error }"
          type="text"
          id="lastName"
          v-model.trim="lastName"
          @input="$v.lastName.$touch()"
        />

        <div v-if="$v.lastName.$dirty">
          <p class="error-message" v-if="!$v.lastName.required">
            Last Name must not be empty.
          </p>
        </div>
      </div>
      <div class="input">
        <label for="password">Password</label>
        <input
          :class="{ error: $v.password.$error }"
          type="password"
          id="password"
          v-model.trim="password"
          @input="$v.password.$touch()"
        />

        <div v-if="$v.password.$dirty">
          <p class="error-message" v-if="!$v.password.required">
            Password must not be empty.
          </p>
        </div>
      </div>
      <div class="input">
        <label for="repeatPassword">Repeat Password</label>
        <input
          :class="{ error: $v.repeatPassword.$error }"
          type="password"
          id="repeatPassword"
          v-model.trim="repeatPassword"
          @input="$v.repeatPassword.$touch()"
        />

        <div v-if="$v.repeatPassword.$dirty">
          <p class="error-message" v-if="!$v.repeatPassword.sameAsPassword">
            Passwords must be identical.
          </p>

          <p class="error-message" v-if="!$v.repeatPassword.required">
            Password must not be empty.
          </p>
        </div>
      </div>

       <div class="input">
        <label for="inputField">Tom test</label>
        <input
          :class="{ error: $v.inputField.$error }"
          type="text"
          id="inputField"
          v-model.trim="inputField"
          @input="$v.inputField.$touch()"
        />

        <div v-if="$v.inputField.$dirty">
          <p class="error-message" v-if="!$v.inputField.TomValidator">
            Must be equal to tom.
          </p>
        </div>
      </div>


       <div class="input">
        <label for="userName">Username async</label>
        <input
          :class="{ error: $v.userName.$error }"
          type="text"
          id="userName"
          v-model.trim="userName"
           @input="$v.userName.$touch()"
        />

        <div v-if="$v.userName.$dirty">
           <p class="error-message" v-if="!$v.userName.required">
            Username is required
          </p>
         
        </div>

        <div v-if="!$v.userName.$pending">
          
          <p class="error-message" v-if="!$v.userName.isUnique">
            This username is already registered.
          </p>
        </div>


      </div>

      

      <button :disabled="$v.$invalid" type="submit">Submit</button>
    </form>

    <div class="validators">
      <pre>{{ $v }}</pre>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import { required, email, minLength, sameAs } from "vuelidate/lib/validators";
import axios from 'axios';


export const TomValidator = (value) => {
  return value === 'tom';
}

export default {
  name: "home",
  components: {
    HelloWorld
  },
  data() {
    return {
      email: "",
      password: "",
      repeatPassword: "",
      firstName: "",
      lastName: "",
      inputField: '',
      userName:'',

    };
  },
  methods: {
    onSubmit() {
      if (!this.$v.$invalid) {
        const user = {
          email: this.email,
          firstName: this.firstName,
          lastName: this.lastName,
          password: this.password,
          repeatPassword: this.repeatPassword,
          inputField: this.inputField,
          userName:this.userName
        };

        console.log(user);

        // Submit the object to an API of sorts
      }
    }
  },
  validations: {
    email: {
      required,
      email
    },
    firstName: {
      required
    },
    lastName: {
      required
    },
    password: {
      required,
      minLength: minLength(6)
    },
    repeatPassword: {
      required,
      minLength: minLength(6),
      sameAsPassword: sameAs("password")
    },
    inputField: {
      TomValidator
    },
    userName:{
      required,
     /* isUnique(value) {
        // standalone validator ideally should not assume a field is required
        if (value === '') return true

        // simulate async call, fail for all logins with even length
        /*return new Promise((resolve, reject) => {
          setTimeout(() => {
            resolve(typeof value === 'string' && value.length % 2 !== 0)
          }, 350 + Math.random() * 300)
        })*/

        /*axios.get(`https://jsonplaceholder.typicode.com/users/1`)
          .then(response => {
            this.posts = response.data['']
        })

      },*/

      async isUnique (value) {
          if (value === '') return true
          //const response = await fetch(`/api/unique/${value}`)
          //return Boolean(await response.json())

          let { data } = await axios.get("https://jsonplaceholder.typicode.com/users/1");

          return data['username'] == value ? false : true;
      }
    }
  }
};
</script>

<style>
.form {
  display: inline-block;
  text-align: center;
  width: 49%;
}
.validators {
  display: inline-block;
  width: 49%;
  text-align: center;
  vertical-align: top;
}
.input {
  padding: 5px;
}

input:focus {
  outline: none;
}
.error {
  border: 1px solid red;
}
</style>

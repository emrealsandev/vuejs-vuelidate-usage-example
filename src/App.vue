<template>
  <div class="container">
    <h3 class="text-center mt-3">Vuelidate ile Form Kontrolü</h3>
    <div class="d-flex justify-content-center align-content-center flex-row">
      <div class="card p-4 mt-3 shadow">
        <form style="width: 350px" @submit.prevent="onSubmit">
          <div class="form-group">
            <label>E-posta Adresiniz</label>
            <input
              @input="$v.email.$touch()"
              v-model="email"
              type="email"
              class="form-control"
              :class="{ 'is-invalid': $v.email.$error }"
              placeholder="E-posta adresini giriniz"
            />
            <small v-if="!$v.email.required" class="form-text text-danger"
              >Bu alan zorunludur...</small
            >
            <small v-if="!$v.email.email" class="form-text text-danger"
              >Geçerli bir e posta giriniz</small
            >
          </div>
          <div class="form-group">
            <label>Şifre</label>
            <input
              v-model="$v.password.$model"
              type="password"
              class="form-control"
              placeholder="Şifrenizi giriniz"
            />
            <small v-if="!$v.password.required" class="form-text text-danger"
              >Bu alan zorunludur...</small
            >
            <small v-if="!$v.password.numeric" class="form-text text-danger"
              >Lütfen şifreniz sayı olsun</small
            >
          </div>
          <div class="form-group">
            <label>Şifre Tekrar</label>
            <input
              v-model="$v.repassword.$model"
              type="password"
              class="form-control"
              placeholder="Şifrenizi tekrar giriniz"
            />
            <small v-if="!$v.repassword.required" class="form-text text-danger"
              >Bu alan zorunludur...</small
            >
            <small v-if="!$v.repassword.numeric" class="form-text text-danger"
              >Lütfen şifreniz sayı olsun</small
            >
            <small v-if="!$v.repassword.sameAs" class="form-text text-danger"
              >Şifreler uyuşmuyor...</small
            >
          </div>
          <div class="form-group">
            <label>Kayıt olmak istediğiniz kategori</label>
            <select v-model="$v.selectedCategory.$model" class="form-control">
              <option
                v-for="category in categories"
                :value="category"
                :key="category"
              >
                {{ category }}
              </option>
            </select>
            <small
              v-if="!$v.selectedCategory.checked"
              class="form-text text-danger"
              >Sadece yazılım seçiniz...</small
            >
          </div>

          <a
            @click="newHobby"
            class="text-white btn btn-secondary rounded-0 btn-sm"
            >İlgi Alanı Ekle</a
          >
          <small v-if="!$v.hobbies.required" class="form-text text-danger"
            >Bu alan zorunludur...</small
          >
          <small v-if="!$v.hobbies.minLength" class="form-text text-danger"
            >2 tane olmalıdır</small
          >
          <ul class="list-group mt-3 mb-3 border-0">
            <li
              v-for="(hobby, index) in hobbies"
              class="list-group-item d-flex pl-2"
              :key="hobby"
            >
              <input
                type="text"
                class="form-control mr-2"
                v-model="hobby.value"
                @blur="$v.hobbies.$each[index].value.$touch"
              />
              <button
                class="btn btn-sm btn-danger rounded-0"
                @click="hobbies.splice(index, 1)"
              >
                Sil
              </button>
            </li>
          </ul>

          <button class="btn btn-outline-secondary rounded-0">Kaydet</button>
        </form>
      </div>
      <div class="card p-4 mt-3 shadow" style="width: 400px">
        <p>{{ $v.password }}</p>
      </div>
    </div>
  </div>
</template>
<script>
import {
  required,
  email,
  numeric,
  minLength,
  maxLength,
  sameAs,
} from "vuelidate/lib/validators";

export default {
  data() {
    return {
      email: null,
      password: null,
      repassword: null,
      selectedCategory: null,
      categories: [
        "Yazılım",
        "Donanım",
        "Cloud",
        "Sunucular",
        "Unix",
        "Linux",
        "Mac OS",
        "Windows",
      ],
      hobbies: [],
    };
  },
  validations: {
    email: {
      required,
      email,
    },
    password: {
      required,
      numeric,
      minLength: minLength(6),
      maxLength: maxLength(8),
    },
    repassword: {
      required,
      numeric,
      minLength: minLength(6),
      maxLength: maxLength(8),
      sameAs: sameAs("password"),
    },
    selectedCategory: {
      checked(val, vm) {
        return vm.selectedCategory === "Yazılım" ? true : false;
      },
    },
    hobbies: {
      required,
      minLength: minLength(2),
      $each : {
        value : {
          required,
          minLength: minLength(6),
        }
      }
    },
  },
  methods: {
    onSubmit() {
      let form = {
        email: this.email,
        password: this.password,
        category: this.category,
        hobbies: this.hobbies,
      };
      console.log(form);
    },
    newHobby() {
      let hobby = {
        id: Math.random() * Math.random() * 1000,
        value: "",
      };
      this.hobbies.push(hobby);
    },
  },
};
</script>

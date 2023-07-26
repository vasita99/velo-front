<template>
  <div class="d-flex">
    <el-menu default-active="1" class="py-4">
      <el-menu-item index="1">
        <span class="h6" @click="userProfile()"
          ><i class="fa-solid fa-user mx-2"></i>Account</span
        >
      </el-menu-item>
      <el-menu-item index="2">
        <span class="h6" @click="userBooking()"
          ><i class="fa-solid fa-bag-shopping mx-2"></i>My bookings</span
        >
      </el-menu-item>
    </el-menu>
    <div v-if="showProfile" class="w-100 m-4 p-4 border rounded">
      <div class="mb-4"><h4>Personal details</h4></div>
      <el-form :model="form" label-width="120px">
        <el-form-item label="First name">
          <el-input v-model="form.first_name" />
        </el-form-item>
        <el-form-item label="Last name">
          <el-input v-model="form.last_name" />
        </el-form-item>
        <el-form-item label="Street">
          <el-input v-model="form.street" />
        </el-form-item>
        <el-form-item label="city">
          <el-input v-model="form.city" />
        </el-form-item>
        <el-form-item label="province">
          <el-input v-model="form.province" />
        </el-form-item>
        <el-form-item label="Country">
          <el-input v-model="form.country" />
        </el-form-item>
        <el-form-item label="mobile">
          <el-input v-model="form.mobile" />
        </el-form-item>
        <el-form-item label="email">
          <el-input v-model="form.email" disabled/>
        </el-form-item>
       
      </el-form>
    </div>

    <!-- booking -->

    <div v-if="showBooking" class="w-100 m-4 p-4 border rounded">
      <div class="mb-4"><h4>Personal details</h4></div>
      <div>
        <h4>bookings....</h4>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { ElLoading } from "element-plus";

export default {
  // eslint-disable-next-line vue/multi-word-component-names
  name: "customerprofile",
  data() {
    return {
      showProfile: true,
      form: {
        first_name: "",
        last_name: "",
        mobile: "",
        street: "",
        city: "",
        province: "",
        country: "",
        email: "",
        password: "",
        user_role: "",
        company_name: "",
        company_email: "",
        company_registration_number: "",
        company_mobile: "",
        company_street: "",
        company_city: "",
        company_province: "",
        company_country: "",
      },
    };
  },
  methods: {
    userProfile() {
      this.showProfile = true;
      this.showBooking = false;
    },

    userBooking() {
      this.showBooking = true;
      this.showProfile = false;
    },

    async getUserDetails() {
      const loading = ElLoading.service({
        lock: true,
        text: "Loading",
        background: "rgba(255, 255, 255, 1)",
      });
      await axios
        .get("https://vendor-valley.onrender.com/profile", {
          params: {
            jwtToken: localStorage.getItem("token"),
          },
        })
        .then((res) => {
          console.log(res.data);
          this.form.first_name =  res.data.first_name;
          this.form.last_name = res.data.last_name;
          this.form.mobile = res.data.mobile;
          this.form.email = res.data.email;
          this.form.city = res.data.city;
          this.form.province = res.data.province;
          this.form.street = res.data.street;
          this.form.country = res.data.country;

          loading.close();
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  mounted() {
    this.getUserDetails();
  },
};
</script>

<style scoped></style>

<template>
  <div class="d-flex">
    <el-menu :default-active="activeNo" class="py-4">
      <el-menu-item index="1">
        <span class="h6" @click="userProfile()"
          ><i class="fa-solid fa-user mx-2"></i>Account</span
        >
      </el-menu-item>
      <el-menu-item index="2">
        <span class="h6" @click="service()"
          ><i class="fa-solid fa-store mx-2"></i>Services</span
        >
      </el-menu-item>
      <el-menu-item index="3">
        <span class="h6" @click="business()"
          ><i class="fa-solid fa-briefcase mx-2"></i>My Business</span
        >
      </el-menu-item>
      <el-menu-item index="4">
        <span class="h6" @click="userBooking()"
          ><i class="fa-solid fa-bag-shopping mx-2"></i>My bookings</span
        >
      </el-menu-item>
    </el-menu>

    <!-- profile -->
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
          <el-input v-model="form.email" disabled />
        </el-form-item>
      </el-form>
    </div>

    <!-- my business -->
    <div v-if="showBusiness" class="w-100 m-4 p-4 border rounded">
      <div class="mb-4"><h4>My Business</h4></div>
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
          <el-input v-model="form.email" disabled />
        </el-form-item>
      </el-form>
    </div>
    <!-- business end -->
    <!-- booking -->

    <div v-if="showBooking" class="w-100 m-4 p-4 border rounded">
      <div class="mb-4"><h4>Bookings</h4></div>
      <div>
        <el-table :data="bookingList" style="width: 100%">
          <el-table-column prop="user">
            <template v-slot="{ row }">
              <div>
                <h6>{{ row.serviceName }}</h6>
              </div>
            </template>
          </el-table-column>
          <el-table-column
            label="Booking ID"
            prop="bookingId"
          ></el-table-column>
          <el-table-column
            prop="startDate"
            label="Start date"
          ></el-table-column>
          <el-table-column prop="endDate" label="End date"></el-table-column>
          <el-table-column
            prop="bookingStatus"
            label="Status"
          ></el-table-column>
          <el-table-column>
            <template v-slot="{ row }">
              <a href="#" @click="openModal(row)">show more</a>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </div>

    <!-- services -->
    <div v-if="showService" class="w-100 m-4 p-4 border rounded">
      <div class="mb-4"><h4>Your services</h4></div>
      <div>
        <button class="btn btn-primary" @click="openAddService()">
          + Add service
        </button>
      </div>
    </div>

    <!-- modal -->
    <modal v-model:show="showModal">
      <div>
        <div>
          <h5>Booking ref #{{ selectedItem.bookingId }}</h5>
        </div>
        <div>
          <p>
            <b>Booking date: {{ selectedItem.bookingDate }}</b>
          </p>
        </div>
        <div>
          <p><b>Service:</b> {{ selectedItem.serviceName }}</p>
        </div>
        <div>
          <p><b>Customer:</b> {{ customer }}</p>
        </div>
      </div>
    </modal>

    <!-- service modal -->
    <modal v-model:show="showAddServiceModal">
      <div class="p-4">
        <el-form :model="form" label-width="120px">
          <el-form-item label="Serive name">
            <el-input v-model="form.serviceName" />
          </el-form-item>
          <el-form-item label="Description">
            <el-input v-model="form.serviceDescription" type="textarea" />
          </el-form-item>

          <el-form-item label="Category">
            <el-select
              v-model="selectedCategories"
              multiple
              placeholder="Categories"
              class="w-25"
            >
              <el-option
                v-for="item in categories"
                :key="item"
                :label="item"
                :value="item"
              />
            </el-select>
          </el-form-item>

          <el-form-item label="Price">
            <el-input v-model="form.servicePrice" class="w-25" />
          </el-form-item>

          <el-form-item label="Upload images">
            <el-upload drag type="file" accept="image/*" multiple ref="myimage">
              <i class="fa fa-cloud-arrow-up" style="color: lightgreen"></i>
              <div class="el-upload__text">
                Drop file here or <em>click to upload</em>
              </div>
              <template #tip>
                <div class="el-upload__tip">
                  Max file size : 5MB. File format : pdf, docx, png, jpeg, ...
                </div>
              </template>
            </el-upload>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" @click="onSubmit">Submit</el-button>
            <el-button @click="showAddServiceModal = false">Cancel</el-button>
          </el-form-item>
        </el-form>
      </div>
    </modal>
  </div>
</template>

<script>
import axios from "axios";
import { ElLoading } from "element-plus";
import Modal from "@/components/Modal.vue";

export default {
  components: { Modal },
  // eslint-disable-next-line vue/multi-word-component-names
  name: "customerprofile",
  data() {
    return {
      myimage: [],
      activeNo: "",
      showService: false,
      showAddServiceModal: false,
      customer: "",
      showModal: false,
      bookingList: [],
      showProfile: true,
      showBooking: false,
      showBusiness: false,
      categories: [],
      selectedCategories: [],
      selectedItem: [],
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
    openAddService() {
      this.showAddServiceModal = true;
    },
    service() {
      this.showService = true;
      this.showProfile = false;
      this.showBooking = false;
      this.showBusiness = false;
    },
    business() {
      this.showBusiness = true;
      this.showBooking = false;
      this.showProfile = false;
      this.showService = false;
    },
    openModal(item) {
      this.selectedItem = item;
      this.customer = this.selectedItem.user.fullName;
      this.showModal = true;
    },
    getBookings() {
      axios
        .get("https://vendor-valley.onrender.com/bookings", {
          params: {
            jwtToken:
              "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc1ZlbmRvciI6MSwidXNlcklkIjo1LCJlbWFpbCI6ImpvaG5kb2UuMjFAZ21haWwuY29tIn0.9Q7uXbfKvgDI3_j9Me4c2z6Pzb6HByF37rLGDgyuTXE",
          },
        })
        .then((res) => {
          console.log(res.data);
          this.bookingList = res.data;
        })
        .catch((err) => {
          console.log(err.message);
        });
    },
    userProfile() {
      this.showProfile = true;
      this.showBooking = false;
      this.showService = false;
      this.showBusiness = false;
    },

    userBooking() {
      this.showBooking = true;
      this.showProfile = false;
      this.showService = false;
      this.showBusiness = false;
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
          this.form.first_name = res.data.first_name;
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
    getCategories() {
      axios
        .get("http://localhost:8080/categories")
        .then((res) => {
          res.data.forEach((element) => {
            this.categories.push(element.categoryName);
          });
          console.log(this.categories);
        })
        .catch((err) => {
          console.log(err.message);
        });
    },
  },
  mounted() {
    this.activeNo = 1;
    const show = this.$route.query.show;
    if (show == "true") {
      this.activeNo = 2;
      this.service();
    }
    this.getUserDetails();
    this.getBookings();
    this.getCategories();
  },
};
</script>

<style scoped></style>

<template>
  <div>
    <div class="block">
      <span class="demonstration">USERS</span>
      <el-slider
          v-model="users_value"
          :marks="users_marks"
          :step="1"
          :max="25"
          :min="1"
          show-stops
          :show-tooltip="false"
      ></el-slider>
    </div>
    <div class="block">
      <span class="demonstration">Security Standard (Set Defaults)</span>
      <el-slider
          v-model="security_standard_value"
          :marks="security_standard_marks"
          :step="1"
          :max="3"
          :min="1"
          show-stops
          :show-tooltip="false"
      ></el-slider>
    </div>
    <div class="block">
      <span class="demonstration">MFA (Enforced if Proactive)</span>
      <el-checkbox
          :value="mfa_checked"
          :true-label="1"
          :false-label="2"
      ></el-checkbox>
    </div>
    <div class="block">
      <span class="demonstration">Storage Requirements</span>
      <el-slider
          v-model="storage_requirements_value"
          :marks="storage_requirements_marks"
          :step="1"
          :max="3"
          :min="1"
          show-stops
          :show-tooltip="false"
      ></el-slider>
    </div>
    <div class="block">
      <span class="demonstration">Hosted Applications</span>
      <el-slider
          v-model="hosted_applications_value"
          :marks="hosted_applications_marks"
          :step="1"
          :max="5"
          :min="1"
          show-stops
          :show-tooltip="false"
      ></el-slider>
    </div>
    <div class="block">
      Monthly Estimate <strong>${{ monthly_estimate }}</strong>
    </div>
    <div class="block">
      Monthly Discounted <strong>${{ months_discounted }}</strong>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    self = this
    return {
      users_value: 1,
      users_marks: self.create_int_marks(1,25),
      security_standard_value: 1,
      security_standard_marks: {1: 'Reactive', 2: 'Proactive', 3: 'Compliance'},
      storage_requirements_value: 1,
      storage_requirements_marks: {1: 'No storage', 2: 'Standard', 3: 'Tired'},
      hosted_applications_value: 1,
      hosted_applications_marks: self.create_int_marks(1,5),
    }
  },
  methods: {
    create_int_marks(min,max){
      let obj = {}
      for (let i = min; i <= max; i++) {
        obj[i] = "" + i;
      }
      return obj
    }
  },
  computed: {
    monthly_estimate: function () {
      return (this.users_value + this.hosted_applications_value + this.security_standard_value + this.storage_requirements_value) * 100
    },
    months_discounted: function () {
      return this.users_value + this.hosted_applications_value + this.security_standard_value + this.storage_requirements_value + this.mfa_checked
    },
    mfa_checked:function (){
      if (this.security_standard_value >= 2) {
        return 1
      }
      return 2
    },
  }
}
</script>
<style scoped>
.block {
  margin: 20px 0 60px 0;
}

.demonstration {
  display: block;
}
</style>

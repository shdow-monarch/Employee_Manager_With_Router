<template>
  <el-row class="form-main-container">
    <el-row class="form-container">
      <el-row class="form-header">
        <h1>{{ isEditable ? $t('formEditTitle') : $t('formAddTitle') }}</h1>
      </el-row>
      <el-form v-if="isLoaded" ref="formData" :rules="formRules" :model="formData">
        <el-row class="personal-information">
          <h4>Personal Information:</h4>
          <el-form-item prop="firstName" :label="$t('firstNameLabel')">
            <el-input v-model="formData.firstName" type="text"></el-input>
          </el-form-item>
          <el-form-item prop="lastName" :label="$t('lastNameLabel')">
            <el-input v-model="formData.lastName" type="text"></el-input>
          </el-form-item>
          <el-form-item prop="dob" :label="$t('dobLabel')">
            <el-date-picker v-model="formData.dob" type="date" placeholder="Date" value-format="dd/MM/yyyy">
            </el-date-picker>
          </el-form-item>
          <el-form-item prop="gender" :label="$t('genderLabel')">
            <el-radio-group v-model="formData.gender">
              <el-radio label="male">Male</el-radio>
              <el-radio label="female">Female</el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item prop="mobile" :label="$t('mobileLabel')">
            <el-input v-model.number="formData.mobile" type="text">
              <template slot="prepend">+91</template>
            </el-input>
          </el-form-item>
          <el-form-item prop="email" :label="$t('emailLabel')">
            <el-input v-model="formData.email" type="text"></el-input>
          </el-form-item>
          <el-form-item prop="address.current" :label="$t('currentAddressLabel')">
            <el-input v-model="formData.address.current" type="textarea" placeholder="Address">
            </el-input>
          </el-form-item>
          <el-form-item prop="address.permanent" :label="$t('permanentAddressLabel')">
            <el-input v-model="formData.address.permanent" type="textarea" placeholder="Address">
            </el-input>
          </el-form-item>
          <el-form-item prop="city" :label="$t('cityLabel')">
            <el-input v-model="formData.city" type="text"></el-input>
          </el-form-item>
          <el-form-item prop="state" :label="$t('stateLabel')">
            <el-select v-model="formData.state" placeholder="State">
              <el-option v-for="(item, index) in states" :key="index" :value="item" :label="item">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item prop="zipCode" :label="$t('zipCodeLabel')">
            <el-input v-model="formData.zipCode" type="text"></el-input>
          </el-form-item>
        </el-row>
        <el-row class="employee-details">
          <h4>Employee Details:</h4>
          <el-form-item prop="employeeDetails.department" :label="$t('departmentLabel')">
            <el-select v-model="formData.employeeDetails.department" placeholder="Select">
              <el-option v-for="(item, index) in departments" :key="index" :value="item" :label="item">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item prop="employeeDetails.position" :label="$t('positionLabel')">
            <el-select v-model="formData.employeeDetails.position" placeholder="Select">
              <el-option v-for="(item, index) in positions" :key="index" :value="item" :label="item">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item prop="employeeDetails.type" :label="$t('typeLabel')">
            <el-select v-model="formData.employeeDetails.type" placeholder="Select">
              <el-option v-for="(item, index) in types" :key="index" :value="item" :label="item">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item prop="employeeDetails.skills" :label="$t('skillsLabel')">
            <el-select v-model="formData.employeeDetails.skills" multiple placeholder=" Select">
              <el-option v-for="(item, index) in skillsList" :key="index" :value="item.name" :label="item.displayName">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item prop="employeeDetails.salary" :label="$t('salaryLabel')">
            <el-input v-model.number="formData.employeeDetails.salary" type="text">
              <template slot="append">PA</template>
            </el-input>
          </el-form-item>
          <el-form-item :label="$t('contractLabel')">
            <el-switch v-model="formData.employeeDetails.isContract" active-color="#13ce66" inactive-color="#ff4949">
            </el-switch>
          </el-form-item>
          <el-row v-if="formData.employeeDetails.isContract" class="is-contract">
            <el-form-item :label="$t('startDateLabel')">
              <el-date-picker v-model="formData.employeeDetails.contract.startDate" type="date" placeholder="Join Date"
                value-format="dd/MM/yyyy">
              </el-date-picker>
            </el-form-item>
            <el-form-item :label="$t('endDateLabel')">
              <el-date-picker type="date" v-model="formData.employeeDetails.contract.endDate" placeholder="End Date"
                value-format="dd/MM/yyyy">
              </el-date-picker>
            </el-form-item>
          </el-row>
        </el-row>
        <el-row class="bank-details">
          <h4>Bank Details:</h4>
          <el-form-item prop="bankDetails.bank" :label="$t('bankNameLabel')">
            <el-input v-model="formData.bankDetails.bank" type="text"></el-input>
          </el-form-item>
          <el-form-item prop="bankDetails.branch" :label="$t('branchNameLabel')">
            <el-input v-model="formData.bankDetails.branch" type="text"></el-input>
          </el-form-item>
          <el-form-item prop="bankDetails.accountNumber" :label="$t('accountNumberLabel')">
            <el-input v-model.number="formData.bankDetails.accountNumber" type="text"></el-input>
          </el-form-item>
          <el-form-item prop="bankDetails.ifsc" :label="$t('ifscLabel')">
            <el-input v-model="formData.bankDetails.ifsc" type="text"></el-input>
          </el-form-item>
        </el-row>
        <el-form-item>
          <el-row class="form-footer">
            <el-button @click="handleCancelClick">{{ $t('cancelButton') }}</el-button>
            <el-button @click="handleSaveClick()" type="success">{{ isEditable ? $t('saveButton') : $t('addButton') }}
            </el-button>
          </el-row>
        </el-form-item>
      </el-form>
    </el-row>
  </el-row>
</template>
<script>
import { v4 as uuidv4 } from 'uuid'
import _ from 'lodash'
import Constants from '@/constants'
export default {
  name: 'EmployeeForm',
  data() {
    return {
      isLoaded: false,
      states: Constants.STATE_LIST,
      skillsList: Constants.SKILL_LIST,
      departments: Constants.DEPARTMENT_LIST,
      positions: Constants.POSITION_LIST,
      types: Constants.EMPLOYEE_TYPE_LIST,
      formRules: Constants.EMPLOYEE_FORM_RULES,
      isEditable: false,
      formData: {},
      tableData: [],
      record: JSON.parse(localStorage.getItem('record'))
    }
  },
  mounted() {
    const record = JSON.parse(localStorage.getItem('record')) || {}
    this.tableData = JSON.parse(localStorage.getItem('employeeList')) || []
    this.formData = !_.isEmpty(record.id) ? _.cloneDeep(record) : Constants.EMPLOYEE_DETAIL
    this.isEditable = !_.isEmpty(record.id)
    this.isLoaded = true
  },
  methods: {
    handleCancelClick() {
      this.$router.push('/')
    },
    handleSaveClick() {
      this.$refs.formData.validate((valid) => {
        if (valid) {
          const payload = _.cloneDeep(this.formData)
          const dataIndex = this.tableData.findIndex(item => item.id === payload.id)
          if (_.isEmpty(payload.id)) {
            payload.id = uuidv4()
          }
          if (dataIndex !== -1) {
            this.tableData.splice(dataIndex, 1, payload)
          } else {
            this.tableData.push(payload)
          }
          localStorage.setItem('employeeList', JSON.stringify(this.tableData))
          this.$message({ type: 'success', message: this.$t('saveMessageStart') + ` ` + `${dataIndex !== -1 ? this.$t('updateMessage') : this.$t('addMessage')}` + ` ` + this.$t('saveMessageEnd') })
          this.$router.push('/')
          this.$refs.formData.resetFields()
        }
        else {
          this.$message({ type: 'error', message: this.$t('formValidationMessage') })
        }
      })
    }
  }
}
</script>

<style scoped>
.el-text-input {
  width: 500px;
}

.el-button.el-button--default {
  margin-left: 320px;
}

.form-main-container {
  display: flex;
  margin: auto;
  place-content: center;
  padding: 30px;
}

.form-container {
  padding-bottom: 35px;
  padding-left: 135px;
  padding-top: 35px;
  padding-right: 135px;
  border: 2px solid black;
}
</style>
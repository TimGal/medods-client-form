<template> 
    <div>
    <div class="form-container">
		<div class="card">
			<div class="bitmap">
				<h1 class="page-header">Запись на приём</h1>
			</div>
			<div class="form-wrapper">
				<h2 v-if="currentTab > 0" class="form-header">Впервые на MedODS?</h2>
				<h2 v-if="currentTab == 0" class="form-header">Клиент создан</h2>
				<h4 v-if="currentTab > 0" class="form-description">Заполните форму снизу чтобы записаться на приём.</h4>
        <h4 v-if="currentTab == 0" class="form-description form-description-created">Ваша запись создана.</h4>
				<form id="ajax_form" class="form-signup" method="post" novalidate>
          <!-- Begin of first tab -->
          <div v-show="currentTab == 1" class="tab">
          <!-- Last name -->
          <div :class="{ 'input-wrapper--error': $v.clientData.last_name.$error }" class='input-container'>
            <Input v-model="$v.clientData.last_name.$model"
                  inputId="last_name" className='input-wrapper'  
                  inputType="text" inputLabel="Фамилия" 
                  inputPlaceholder="Введите фамилию" 
            ></Input>
            <div class="input-error" v-show="!$v.clientData.last_name.required && $v.clientData.last_name.$dirty">{{ requiredErrorText }}</div>
           </div> 
					<!-- First name -->
          <div class="input-container" :class="{ 'input-wrapper--error': $v.clientData.first_name.$error }">
          <Input v-model="$v.clientData.first_name.$model" 
                 inputId="first_name" inputType="text" 
                 inputLabel="Имя" inputPlaceholder="Введите имя"></Input>
          <div class="input-error" v-show="!$v.clientData.first_name.required && $v.clientData.first_name.$dirty">{{ requiredErrorText }}</div>
          </div> 
          <!-- Patronymic -->
          <div class="input-container">
          <Input v-model="clientData.patronymic"  inputId="patronymic" inputType="text" inputLabel="Отчество" inputPlaceholder="Введите отчество"></Input>
          </div> 
          <!-- Date of Birth -->
          <div class="input-container" :class="{ 'input-wrapper--error': $v.clientData.date_of_birth.$error }">
          <Input v-model="$v.clientData.date_of_birth.$model"  
                 inputId="date_of_birth" inputType="date" inputLabel="Дата рождения"
                 ></Input>
          <div class="input-error" v-show="!$v.clientData.date_of_birth.required && $v.clientData.date_of_birth.$dirty">{{ requiredErrorText }}</div>
          </div> 
          <!-- Phone number -->
          <div class="input-container" :class="{ 'input-wrapper--error': $v.clientData.phone_number.$error }">
          <Input v-model="$v.clientData.phone_number.$model" 
                 inputId="phone_number" inputType="tel" inputLabel="Номер телефона" 
                 inputPlaceholder="Введите номер телефона"></Input>
          <div class="input-error" v-show="!$v.clientData.phone_number.required && $v.clientData.phone_number.$dirty">{{ requiredErrorText }}</div>
          <div class="input-error" v-show="!$v.clientData.phone_number.mustBePhoneNumber && $v.clientData.phone_number.required">Должно иметь 11 цифр и начинаться на 7</div>
          </div> 
          <!-- Gender -->
          <div class="input-container">
          <Input v-model="clientData.gender" 
                 className="input-wrapper gender"
                 inputId="gender"
                 inputType="radio"
                 inputLabel="Пол"
                 classNameWrapper="gender-item-wrapper"
                 labelClassname="radio-label"
                 :nameOfList="genderList"              
                 ></Input>
          </div> 
					<!-- Client group -->
          <div class="input-container">
          <Input v-model="$v.clientData.client_group.$model" 
                  inputId="client_group" inputType="select-multiple" 
                 inputLabel="Группа клиентов" :nameOfList="clientGroup"></Input>
          <div class="input-error" v-show="!$v.clientData.client_group.required && $v.clientData.client_group.$dirty">{{ requiredErrorText }}</div>
          </div> 
          <!-- Attending doctor -->
          <div class="input-container">
          <Input v-model="clientData.attending_doctor" inputId="attending_doctor" inputType="select" inputLabel="Лечащий доктор" firstOptionForSelect="Выберите доктора" :nameOfList="doctorList"></Input>
          </div> 
          <!-- Not sending SMS -->
          <div class="input-container">
          <Input v-model="clientData.sending_SMS" className="input-wrapper"
                 inputId="sending_SMS" 
                 inputType="checkbox" 
                 inputLabel="Не отправлять СМС" 
                 classNameWrapper="custom-checkbox"
                 checkboxValue="isNotSending"
                 ></Input>
          </div> 
          </div>
          <!-- End of first tab -->
          
          <!-- Begin of second tab -->
          <div v-show="currentTab == 2" class="tab">
            <!-- City_index -->
            <div class="input-container">
            <Input v-model="clientData.city_index" inputId="city_index" inputType="text" inputLabel="Индекс" inputPlaceholder="Введите индекс"></Input>
           </div>
            <!-- Country -->
            <div class="input-container">
            <Input v-model="clientData.country" inputId="country" inputType="text" inputLabel="Страна" inputPlaceholder="Введите название страны"></Input>
            </div>
            <!-- Region -->
            <div class="input-container">
            <Input v-model="clientData.region" inputId="region" inputType="text" inputLabel="Область" inputPlaceholder="Введите навание области"></Input>
            </div>
            <!-- City name -->
            <div class="input-container" :class="{ 'input-wrapper--error': $v.clientData.city_name.$error }">
              <Input v-model="$v.clientData.city_name.$model" inputId="city_name" inputType="text" inputLabel="Город" inputPlaceholder="Введите навание города"></Input>
            <div class="input-error" v-show="!$v.clientData.city_name.required && $v.clientData.city_name.$dirty">{{ requiredErrorText }}</div>
            </div>
            <!-- Street name -->
            <div class="input-container">
            <Input v-model="clientData.street_name" inputId="street_name" inputType="text" inputLabel="Улица" inputPlaceholder="Введите название улицы"></Input>
            </div>
            <!-- House number -->
            <div class="input-container">
            <Input v-model="clientData.house_number" inputId="house_number" inputType="text" inputLabel="Дом" inputPlaceholder="Введите номер дома"></Input>
            </div>
          </div>
          <!-- End of second tab -->

          <!-- Begin of third tab -->
          <div v-show="currentTab == 3" class="tab">
            <!-- document Type -->
            <div class="input-container">
            <Input v-model="$v.clientData.documentType.$model" inputId="documentType" inputType="select" inputLabel="Тип документа" firstOptionForSelect="Выберите документ" :nameOfList="documentList"></Input>
            <div class="input-error" v-show="!$v.clientData.documentType.required && $v.clientData.documentType.$dirty">{{ requiredErrorText }}</div>
            </div>
            <!-- document_series -->
            <div class="input-container">
            <Input v-model="clientData.document_series" inputId="document_series" inputType="text" inputLabel="Серия" inputPlaceholder="Введите серию документа"></Input>
            </div>
            <!-- document_number -->
            <div class="input-container">
            <Input v-model="clientData.document_number" inputId="document_number" inputType="text" inputLabel="Номер" inputPlaceholder="Введите номер документа"></Input>
            </div>
            <!-- whom_issued_by -->
            <div class="input-container">
            <Input v-model="clientData.whom_issued_by" inputId="whom_issued_by" inputType="text" inputLabel="Кем выдан" inputPlaceholder="Введите кем выдан документ"></Input>
            </div>
            <!-- document_date_of_issue -->
            <div class="input-container">
            <Input v-model="$v.clientData.document_date_of_issue.$model" inputId="document_date_of_issue" inputType="date" inputLabel="Дата выдачи"></Input>
            <div class="input-error" v-show="!$v.clientData.document_date_of_issue.required && $v.clientData.document_date_of_issue.$dirty">{{ requiredErrorText }}</div>
            </div>
          </div>
          <!-- End of third tab -->
					
					<div class="btn-submit-wrapper">
            <button v-if="currentTab == maxTabs" @click="createClient" id="btn-complete" type="button" class="btn-submit">Записаться</button>
            <button v-if="currentTab < maxTabs && currentTab != 0" @click="nextTab(currentTab)" id="btn-complete" type="button" class="btn-submit">Далее</button>
						<button v-if="currentTab > 1" @click="prevTab" id="btn-complete" type="button" class="btn-submit">Назад</button>
					</div>
				</form>

				<div id="draw-svg" class="person-svg-wrapper"></div>

			</div>
      
      </div>
    </div>
    {{ clientData }}
    <br>
    {{ "$v.clientData.phone_number.mustBePhoneNumber :" + $v.clientData.phone_number.mustBePhoneNumber}}
    <br>
    {{ "$v.validationGroup.$anyError :" + $v.validationGroup1.$anyError }}
    <br>
    {{ "$v.clientData.client_group :" + $v.clientData.client_group.required }}
    <br>
    {{ "$v.clientData.documentType :" + $v.clientData.documentType.required + ' ' +  $v.clientData.documentType.$dirty}}
    <br>
    {{ "$v.clientData.document_date_of_issue :" + $v.clientData.document_date_of_issue.required + ' ' +  $v.clientData.document_date_of_issue.$dirty }}
    </div>
</template>

<script>
import Input from './Input.vue'
import { required } from 'vuelidate/lib/validators';

const mustBePhoneNumber = function(number) {
  return number.indexOf('7') == 0 && number.length == 11
}

// const checkFirstTab = function() {
//   console.log(this.$v);
// }

export default {
  components: { Input },
  name: 'ClientForm',
  data() {
    return { 
        currentTab: 1,
        maxTabs: 3,
        doctorList: [
          {ivanov: "Иванов"},
         {zaharov: "Захаров"},
          {chernysheva: "Чернышева"},
        ],
        genderList: [
          {male: "Мужской",},
          {female: "Женский",},
        ],
        clientGroup: [
          {vip: "VIP"},
          {problem: "Проблемные"},
          {oms: "ОМС"},
        ],
        documentList: [
          {passport: "Паспорт"},
          {birthCertificate: "Свидетельство о рождении"},
          {driverLicense: "Водительское удостоверение"}
        ],
        clientData: {
          last_name: '',
          first_name: '',
          patronymic: '',
          date_of_birth: '',
          phone_number: '',
          gender: '',
          client_group: '',
          attending_doctor: '',
          sending_SMS: false,
          city_index: '',
          country: '',
          region: '',
          city_name: '',
          street_name: '',
          house_number: '',
          documentType: '',
          document_series: '',
          document_number: '',
          whom_issued_by: '',
          document_date_of_issue: '',
        },
        requiredErrorText : 'Поле обязательное для заполнения',

    }
  },
  methods: {
    nextTab: function(step) {
      let currentValidationGroup;
      if (step == 1) currentValidationGroup = this.$v.validationGroup1
      if (step == 2) currentValidationGroup = this.$v.validationGroup2
      currentValidationGroup.$touch();
      if (!currentValidationGroup.$anyError) this.currentTab += 1
      
    },
    prevTab: function() {
      this.currentTab -= 1;
    },
    createClient: function() {
      
      this.$v.validationGroup3.$touch();
      if (!this.$v.validationGroup3.$anyError) this.currentTab = 0
    },

  },
  computed: {
    disabledBtn: function() {
      return this.$v.clientData.last_name.$invalid ||
             this.$v.clientData.first_name.$invalid
    },
  },
  validations: {
    clientData: {
      last_name: {
        required,
      },
      first_name: {
        required,
      },
      date_of_birth: {
        required,
      },
      phone_number: {
        required,
        mustBePhoneNumber,
      },
      client_group: {
        required,
      },
      city_name: {
        required,
      },
      documentType: {
        required,
      },
      document_date_of_issue: {
        required,
      },
    },
    validationGroup1: ['clientData.last_name', 'clientData.first_name','clientData.phone_number', 'clientData.date_of_birth','clientData.client_group'],
    validationGroup2: ['clientData.city_name'],
    validationGroup3: ['clientData.documentType','clientData.document_date_of_issue'],
    }


  

}
</script>

<style lang="sass">
@import './ClientForm'
</style>
<template>
  <div :class="'input-wrapper'">
  <template v-if="inputType == 'text'" >
		<label :for="inputId">{{ inputLabel }}</label>
		<input :id="inputId" :type="inputType" :name="inputId" :placeholder="inputPlaceholder" :value="modelValue" @input="$emit('input', $event.target.value)">
	</template>
  
  <template v-else-if="inputType == 'date'">
		<label :for="inputId">{{ inputLabel }}</label>
		<input :id="inputId" :type="inputType" :name="inputId" :value="modelValue" @input="$emit('input', $event.target.value)"  min="1900-01-01" :max="currentDate"/>
	</template>

  <template v-else-if="inputType == 'tel'">
		<label :for="inputId">{{ inputLabel }}</label>
		<input :id="inputId" :type="inputType" :name="inputId" :placeholder="inputPlaceholder" :value="modelValue" @input="$emit('input', $event.target.value)">
	</template>

  <template v-else-if="inputType == 'radio' && inputId == 'gender'">
		<label :for="inputId">{{ inputLabel }}</label>
    <div class="gender-input-container">
    <template v-for="gender in nameOfList">
      <div v-for="(value, key) in gender" :key="key" :class="classNameWrapper">
			<label :class="labelClassname"><input :class="inputId + '-input'" :type="inputType" :name="inputId" :value="key" @input="$emit('input', $event.target.value)">
        <div :class="inputId + '-' + inputType"></div>{{ value }}
			</label>
		</div>
    </template>
    </div>
	</template>

  <template v-else-if="inputType == 'select-multiple'">
		<label :for="inputId">{{ inputLabel }}</label>
		<select :class="inputType" :id="inputId" :name="inputId + '[]'" :value="value" @input="$emit('input', $event.target.value)" size="3" multiple>
      <template v-for="client in nameOfList">
        <option v-for="(value, key) in client" :value="key" :key="key">{{ value }}</option>
      </template>  
		</select>
	</template>

  <template v-else-if="inputType == 'select'">
		<label :for="inputId">{{ inputLabel }}</label>
		<select :id="inputId" :name="inputId" :value="value" @input="$emit('input', $event.target.value)">
			<option value="" disabled selected>{{ firstOptionForSelect }}</option>
      <template v-for="item in nameOfList">
        <option v-for="(value, key) in item" :key="key" :value="key">{{ value }}</option>  
      </template>
		</select>
	</template>

  <template v-else-if="inputType == 'checkbox'" >
    <input :type="inputType" :class="classNameWrapper" :id="inputId" :name="inputId" :value="value" @input="$emit('input', $event.target.checked)">
    <label :for="inputId">{{ inputLabel }}</label>
  </template>
  
  </div>
</template>

<script>
function getCurrentDate() {
  return `${new Date().getYear() + 1900}-${(new Date().getMonth() + 1) < 10 ? "0" + (new Date().getMonth() + 1) : (new Date().getMonth() + 1)}-${new Date().getDate() < 10 ? "0" + new Date().getDate() : new Date().getDate()}`
}
export default {
  props:[
    'modelValue',
    'value',
    'className',
    'inputId',
    'inputType',
    'inputPlaceholder',
    'inputLabel',
    'classNameWrapper',
    'labelClassname',
    'firstOptionForSelect',
    'checkboxValue',
    'nameOfList',

  ],
  data(){
    return { 
      currentDate: getCurrentDate(),
      
    }
  },

}
</script>

<style lang="sass">

</style>

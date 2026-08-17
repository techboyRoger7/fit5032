<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <h1 class="text-center">User Information Form / Credentials</h1>
                <form @submit.prevent="submitForm">
                    <div class="row mb-3">
                        <div class="col-md-6">
                            <label for="username" class="form-label">Username</label>
                            <input type="text" id="username" class="form-control" v-model="formData.username" @blur="() => validateName(true)" @focus="() => validateName(false)">
                            <div v-if="errors.username" class="text-danger">{{ errors.username }}</div>
                        </div>
                       <div class="col-md-6">
                            <label for="password" class="form-label">Password</label>
                            <input type="text" id="password" class="form-control" v-model="formData.password" @blur="() => validatePassword(true)" @focus="validatePassword(false)">
                            <div v-if="errors.password" class="text-danger">{{ errors.password }}</div>                    
                        </div>                        
                    </div>
                    <div class="row mb-3">
                        <div class="col-md-6">
                            <div class="form-check">
                                <label for="checkbox" class="form-label">Australian citizen?</label>
                                <input type="checkbox" id="isAustralian" class="form-check-input" v-model="formData.isAustralia"/>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <label for="gender" class="form-label">Gender</label>
                            <select id="gender" class="form-select" v-model="formData.gender" @focus="() => validateGender(false)" @blur="() => validateGender(true)">
                                <option value="male">Male</option>
                                <option value="female">Female</option>
                                <option value="other">Other</option>
                            </select>
                            <div v-if="errors.gender" class="text-danger">{{ errors.gender }}</div>
                        </div>                       
                    </div>
                    <div class="mb-3">
                        <label for="reasonForLearning" class="form-label">Reason for joining</label>
                        <textarea id="reasonForJoining" name="reasonForJoining" row="3" class="form-control" v-model="formData.reason"  @blur="() => validateReason(true)" @focus="() => validateReason(false)"></textarea>
                        <div v-if="errors.reason" class="text-danger"> {{ errors.reason }}</div>                        
                    </div>
                    <div class="text-center">
                        <button type="submit" class="btn btn-primary me-2">Submit</button>
                        <button type="button" class="btn btn-secondary" @click="clearForm">Clear</button>
                    </div>
                </form>
                <div class="row mt-5 text-center" v-if="submittedCards.length">
                <div class="d-flex flex-wrap justify-content-start">
                
                    <DataTable :value="submittedCards" class="col-md-12">
                        <Column field="username" header="Username">{{ card.username }}</Column>
                        <Column field="resident" header="Australian Resident">{{ card.resident }}</Column>
                        <Column field="gender" header="Gender">{{ card.gender }}</Column>
                        <Column field="reason" header="Reason">{{ card.reason }}</Column>

                    </DataTable>
                        <!-- <div class="card-header">
                            User Information
                        </div>
                        <ul class="list-group list-group-flush">
                            <li class="list-group-item">Username: {{ card.username }}</li>
                            <li class="list-group-item">Password: {{ card.password }}</li>
                            <li class="list-group-item">Australian Resident: {{ card.isAustralian ? 'Yes' : 'No' }}</li>
                            <li class="list-group-item">Gender: {{ card.gender }}</li>
                            <li class="list-group-item">Reason: {{ card.reason }}</li>
                        </ul> -->
           
                </div>
                </div>                
            </div>
        </div>
    </div>
 
        

</template>

<script setup>
// code
import { ref } from 'vue';
import DataTable from 'primevue/datatable';
import Column from 'primevue/column';
  
const formData = ref({
    username: '',
    password: '',
    isAustralian: false,
    reason: '',
    gender: ''
});

const submittedCards = ref([]);

const submitForm = () => {
    validateName(true);
    validatePassword(true);
    validateGender(true);
    validateReason(true);
    if (!errors.value.username) {
        submittedCards.value.push({
            ...formData.value
        });
        clearForm()        
    }
};

const clearForm = () => {
    formData.value = {
        username: '',
        password: '',
        isAustralian: false,
        reason: '',
        gender: ''
    };
};

const errors = ref(
    {
        username: null,
        password: null,
        resident: null,
        gender: null,
        reason: null,
    }
)

// username validation function 
// blur opposite to focus, when input box is blur check username length
const validateName = (blur) => {
    if (formData.value.username.length < 3) {
        if (blur) {
            errors.value.username = "Name must be at least 3 characters";
        } else {
            errors.value.username = null;
        }
    }
}

// validation password
// rules min 8, At least 1 upper and lower case, 1 special char, 1 number
const validatePassword = (blur) => {
    const password = formData.value.password
    const minLength = 8
    const hasUpperCase = /[A-Z]/.test(password)
    const hasLowerCase = /[a-z]/.test(password)
    const hasDigit = /\d/.test(password)
    const hasSpecialChar = /[!@#$%^&*(),.?":{}|<>]/.test(password)

    if (password.length < minLength) {
        if(blur) errors.value.password = "Password must be at least 8 characters"
    } else if (!hasUpperCase) {
        if(blur) errors.value.password = "Password must contain at least 1 upper case character"
    } else if (!hasLowerCase) {
        if(blur) errors.value.password = "Password must contain at least 1 lower case character"
    } else if (!hasSpecialChar) {
        if(blur) errors.value.password = "Password must contain at least 1 special character"
    } else if (!hasDigit) {
        if(blur) errors.value.password = "Password must contain at least 1 digit"
    } else {
        if(blur) errors.value.password = null
    }

}

// validate gender
// must select
const validateGender = (blur) => {
    const gender = formData.value.gender;
    
    if (!gender) {
        if (blur) errors.value.gender = "Must select a gender";
    } else {
        errors.value.gender = null;
    }
}

// validate reason
// No longer than 300 characters
const validateReason = (blur) => {
    const reason = formData.value.reason;
    const maxLength = 300

    if (reason.length > maxLength) {
        if (blur) errors.value.reason = `Reason must not over ${maxLength} characters`
    } else {
        errors.value.reason = null
    }

}


</script>

<style scoped>
   .card {
   border: 1px solid #ccc;
   border-radius: 10px;
   box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
   }
   .card-header {
   background-color: #275FDA;
   color: white;
   padding: 10px;
   border-radius: 10px 10px 0 0;
   }
   .list-group-item {
   padding: 10px;
   }
</style>

<template>
    <div class="container mt-5">
        <div class="row">
            <div class="col-md-12">
                <h1 class="text-center">User Information Form / Credentials</h1>
                <form @submit.prevent="submitForm">
                    <div class="row mb-3">
                        <div class="col-md-6">
                            <label for="username" class="form-label">Username</label>
                            <input type="text" id="username" class="form-control" v-model="formData.username" required>
                        </div>
                       <div class="col-md-6">
                            <label for="password" class="form-label">Password</label>
                            <input type="text" id="password" class="form-control" v-model="formData.password" minlength="4" maxlength="10" required>                    
                        </div>                        
                    </div>
                    <div class="row mb-3">
                        <div class="col-md-6">
                            <div class="form-check">
                                <label for="checkbox" class="form-label">Australian citizen?</label>
                                <input type="checkbox" id="isAustralian" class="form-check-input" v-model="formData.isAustralia" required/>
                            </div>
                        </div>
                        <div class="col-md-6">
                            <label for="gender" class="form-label">Gender</label>
                            <select id="gender" class="form-select" v-model="formData.gender" required>
                                <option value="male">Male</option>
                                <option value="female">Female</option>
                                <option value="other">Other</option>
                            </select>
                        </div>                       
                    </div>
                    <div class="mb-3">
                        <label for="reasonForLearning" class="form-label">Reason for joining</label>
                        <textarea id="reasonForJoining" name="reasonForJoining" row="3" class="form-control" v-model="formData.reason" maxlength="200"></textarea>                        
                    </div>
                    <div class="text-center">
                        <button type="submit" class="btn btn-primary me-2">Submit</button>
                        <button type="button" class="btn btn-secondary" @click="clearForm">Clear</button>
                    </div>
                </form>
                <div class="row mt-5 text-center" v-if="submittedCards.length">
                <div class="d-flex flex-wrap justify-content-start">
                    <div v-for="(card, index) in submittedCards" :key="index" class="card m-2" style="width: 18rem;">
                        <div class="card-header">
                            User Information
                        </div>
                        <ul class="list-group list-group-flush">
                            <li class="list-group-item">Username: {{ card.username }}</li>
                            <li class="list-group-item">Password: {{ card.password }}</li>
                            <li class="list-group-item">Australian Resident: {{ card.isAustralian ? 'Yes' : 'No' }}</li>
                            <li class="list-group-item">Gender: {{ card.gender }}</li>
                            <li class="list-group-item">Reason: {{ card.reason }}</li>
                        </ul>
                    </div>
                </div>
                </div>                
            </div>
        </div>
    </div>
 
        

</template>

<script setup>
// code
import { ref } from 'vue';
  
const formData = ref({
    username: '',
    password: '',
    isAustralian: false,
    reason: '',
    gender: ''
});

const submittedCards = ref([]);

const submitForm = () => {
    submittedCards.value.push({
        ...formData.value
    });
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

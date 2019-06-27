<template>
   <div id = "employee-form">
    <form v-on:submit.prevent = "handleSubmit">
        <label>EmployeeName</label>
        <input 
            ref="first"
            :class="{'has-error':submitting && invalidName}" 
            v-model="employee.name" 
            type="text"
            @focus="clearStatus"
            @keypress="clearStatus"
        />
        <label>EmployeeEmail</label>
        <input 
            :class="{'has-error': submitting && invalidEmail}"
            v-model="employee.email" 
            @focus="clearStatus"
            type="text"
        />
        <p v-if="error && submitting" class="error-message">
            ❗please fill out all required fields
        </p>
        <p v-if="success" class="success-message">
            ✅ Employee successfully added
        </p>

        <button>Add Employee</button>
    </form>   
    </div> 
</template>

<script>
export default {
    name: 'employee-form',
    data() {
        return {
            submitting: false,
            error: false,
            success: false,
            employee: {
                name: '',
                email: '',
            },
        }
    },
    methods: {
        handleSubmit() {
            console.log ('testing handleSubmit')
            this.submitting = true
            this.clearStatus()

            if (this.invalidName || this.invalidEmail){
                this.error = true
                return
            }
            this.$emit('add:employee',this.employee)
            this.$refs.first.focus()
            this.employee = {
                name: '',
                email: '',
            }
            this.error = false
            this.success = true
            this.submitting = false
        },
        clearStatus() {
            this.success = false
            this.error = false
        }
    },
    computed: {
        invalidName() {
            return this.employee.name === '';
        },
        invalidEmail() {
            return this.employee.email ==='';
        },
    }
}
</script>

<style scoped>
    form {
        margin-bottom: 2rem;
    }

    [class*='-message'] {
        font-weight: 500;
    }

    .error-message {
        color: #d33c40;
    }
    
    .success-message {
        color: #32a95d;
    }
</style>

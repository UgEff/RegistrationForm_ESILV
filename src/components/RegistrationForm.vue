<template>
<form @submit.prevent="submitForm">
    <div class="form-group">
        <label for="name">Nom d'utilisateur</label>
        <input v-model="user.name" type="text" id="name" @blur="validateName" />
        <p v-if="errors.name" class="error">{{ errors.name }}</p>
    </div>
    <div class="form-group">
        <label for="email">Email</label>
        <input v-model="user.email" type="email" id="email" @blur="validateEmail" />
        <p v-if="errors.email" class="error">{{ errors.email }}</p>
    </div>
    <div class="form-group">
        <label for="password">Mot de passe</label>
        <input v-model="user.password" type="password" id="password" />
    </div>
    <div class="form-group">
        <label for="confirmPassword">Confirmer le mot de passe</label>
        <input v-model="confirmPassword" type="password" id="confirmPassword" @blur="validatePassword" />
        <p v-if="errors.password" class="error">{{ errors.password }}</p>
    </div>
    <button type="submit" :disabled="!isFormValid">S'inscrire</button>
</form>
</template>

<script>
export default {
data() {
    return {
    user: {
        name: '',
        email: '',
        password: '',
    },
    confirmPassword: '',
    errors: {
        name: null,
        email: null,
        password: null,
    },
    };
},

computed: {
    isFormValid() {
    return (
        !this.errors.name &&
        !this.errors.email &&
        !this.errors.password &&
        this.user.name &&
        this.user.email &&
        this.user.password &&
        this.confirmPassword
    );
    },
},

methods: {

    validateName() {
    if (this.user.name.length < 3) {
        this.errors.name = 'Le nom d\'utilisateur doit contenir au moins 3 caractères.';
    }else{
        const regex = /^[^\s@]+$/; // Pas d'espace
        this.errors.name = regex.test(this.user.name) ? null : 'Le nom ne doit pas contenir d\'espaces ou de caractères spéciaux.';
    }
    },

    validateEmail() {
    const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/; //Format email
    this.errors.email = regex.test(this.user.email) ? null : 'Email invalide.';
    },

    validatePassword() {
    this.errors.password = this.user.password === this.confirmPassword ? null : 'Les mots de passe ne correspondent pas.';
    },

    submitForm() {
    this.validateName();
    this.validateEmail();
    this.validatePassword();

    if (!this.errors.name && !this.errors.email && !this.errors.password) {
        console.log('Formulaire soumis:', this.user);
    }
    }
}
};
</script>

<style>

    body{
        background-color: #000;
    }

    form {
    max-width: 800px;
    margin: 60px auto;
    padding: 30px;
    border: 1px solid #333;
    border-radius: 5px;
    background-color: #1a1a1a;
    color: #fff;
}

.form-group {
    margin-bottom: 15px;
}

label {
    display: block;
    margin-bottom: 5px;
    font-weight: bold;
    color: #ccc; 
}


input[type="text"],input[type="email"],input[type="password"] {
    width: 100%;
    padding: 12px;
    margin-top: 8px;
    border: 2px solid #555; 
    background-color: #333; 
    color: #fff;
    border-radius: 4px;
    box-sizing: border-box;
}


button[type="submit"] {
    width: 100%;
    background-color: #ffcc00;
    color: #000;
    padding: 12px 20px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    font-size: 18px;
}

button[type="submit"]:hover {
    background-color: #e6b800;
}


.error {
    color: #ff6666;
    font-family: Arial, sans-serif;
}

/* Amélioration visuelle lors du focus sur un champ */
input[type="text"]:focus,
input[type="email"]:focus,
input[type="password"]:focus {
    border-color: #4a90e2;
    outline: none;
}
</style>


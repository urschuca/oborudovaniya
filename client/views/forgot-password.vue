<style lang="scss">

#forgotPassword {
    width: 620px;
}

</style>

<template>
    <div id="forgotPasswordContainer">
        <modal id="forgotPassword" :shown="true" :blackout="true">
            <div class="columns">
                <div class="lpHalf">
                    <h3>
                        Mot de passe oublié ?
                    </h3>

                    <p>Veuillez renseigner votre nom utilisateur</p>
                    <form class="forgotPassword" @submit.prevent="resetPassword">
                        <div class="lpFields">
                            <input v-model="forgotPasswordUsername" type="text" placeholder="Username" name="username" class="username">
                            <input type="submit" value="Submit" class="lpButton">
                        </div>

                        <errors :errors="forgotPasswordErrors" />
                    </form>
                </div>
                <div class="lpHalf">
                    <h3>
                        Nom d'utilisateur oublié ?
                    </h3>

                    <p>Veuillez renseigner votre adresse email.</p>
                    <form class="forgotUsername" @submit.prevent="forgotUsername">
                        <div class="lpFields">
                            <input v-model="forgotUsernameEmail" type="text" placeholder="Email Address" name="email" class="email">
                            <input type="submit" value="Submit" class="lpButton">
                        </div>

                        <errors :errors="forgotUsernameErrors" />
                    </form>
                </div>
                <router-link to="/signin" class="lpHref">
                    &larr; Retour à l'écran de connexion
                </router-link>
            </div>
        </modal>
        <blackoutFooter />
    </div>
</template>

<script>
import blackoutFooter from '../components/blackout-footer.vue';
import errors from '../components/errors.vue';
import modal from '../components/modal.vue';

export default {
    name: 'ForgotPassword',
    components: {
        blackoutFooter,
        errors,
        modal,
    },
    data() {
        return {
            forgotPasswordUsername: '',
            forgotPasswordErrors: [],
            forgotUsernameEmail: '',
            forgotUsernameErrors: [],
        };
    },
    methods: {
        resetPassword() {
            this.forgotPasswordErrors = [];

            return fetchJson('/forgotPassword', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                credentials: 'same-origin',
                body: JSON.stringify({ username: this.forgotPasswordUsername }),
            })
                .then((response) => {
                    router.push('/signin/reset-password');
                })
                .catch((response) => {
                    let errors = [{ message: 'An error occurred, please try again later.' }];
                    if (response.json && response.json.errors) {
                        errors = response.json.errors;
                    }
                    this.forgotPasswordErrors = errors;
                });
        },
        forgotUsername() {
            this.forgotUsernameErrors = [];

            return fetchJson('/forgotUsername', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                credentials: 'same-origin',
                body: JSON.stringify({ email: this.forgotUsernameEmail }),
            })
                .then((response) => {
                    router.push('/signin/forgot-username');
                })
                .catch((response) => {
                    let errors = [{ message: 'An error occurred, please try again later.' }];
                    if (response.json && response.json.errors) {
                        errors = response.json.errors;
                    }
                    this.forgotUsernameErrors = errors;
                });
        },
    },
};
</script>

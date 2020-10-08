<style lang="scss">

</style>

<template>
    <div id="registerContainer">
        <modal id="register" :shown="true" :blackout="true">
            <div class="lpModalHeader">
                <h2>
                    Créer un compte
                </h2>
                <router-link to="/signin" class="lpHref">
                    Déjà enregistré ?
                </router-link>
            </div>

            <p v-if="isLocalSaving" class="lpWarning">
                <strong>Note:</strong> Vos données existantes <strong>seront</strong> sauvegardés dans votre compte.
            </p>

            <registerForm />
        </modal>

        <blackoutFooter />
        <globalAlerts />
    </div>
</template>

<script>
import blackoutFooter from '../components/blackout-footer.vue';
import globalAlerts from '../components/global-alerts.vue';
import modal from '../components/modal.vue';
import registerForm from '../components/register-form.vue';

export default {
    name: 'Register',
    components: {
        blackoutFooter,
        globalAlerts,
        modal,
        registerForm,
    },

    computed: {
        isLocalSaving() {
            return this.$store.state.saveType === 'local';
        },
    },
};
</script>

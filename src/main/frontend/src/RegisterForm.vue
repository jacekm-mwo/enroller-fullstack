<template>
  <form @submit.prevent="enter()">
    <label>Register</label>
    <input type="text" v-model="user.login">
    <label>HasĹ‚o</label>
    <input type="password" v-model="user.password">
    <button type="submit">{{ labelOfTheButton }}</button>
  </form>
</template>

<script>
    export default {
        props: ["buttonLabel"],
        data() {
            return {
                user: {}
            };
        },
        methods: {
            enter() {
                this.$emit("login", this.user);
            },
            
	}
            
        },
        computed: {
            labelOfTheButton() {
                return this.buttonLabel || 'zarejestruj sie™';
            }
        }
    };
</script>


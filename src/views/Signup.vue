<template>
	<v-container fluid fill-height>
		<v-layout align-center justify-center>
			<v-flex xs12 sm8 md6>
				<v-card class="elevation-12">
					<v-toolbar dark color="primary">
						<v-toolbar-title>Регистрация</v-toolbar-title>
					</v-toolbar>
					<v-card-text>
						<v-alert :value="error" type="warning">
							{{error}}
						</v-alert>
						<v-form v-model="valid">
							<v-text-field
								prepend-icon="person"
								name="login"
								label="Е-мейл"
								type="email"
								v-model="email"
                :rules="emailRules"
								required
							></v-text-field>
							<v-text-field
								id="password"
								prepend-icon="lock"
								name="password"
								label="Пароль"
								type="password"
								v-model="password"
                :rules="passwordRules"
								required
							></v-text-field>
						</v-form>
					</v-card-text>
					<v-card-actions>
						<v-spacer></v-spacer>
						<v-btn color="primary" @click.prevent="signup" :disabled="processing || !valid"
							>Зарегистрироваться</v-btn
						>
					</v-card-actions>
				</v-card>
			</v-flex>
		</v-layout>
	</v-container>
</template>
<script>
export default {
	data() {
		return {
			email: null,
			password: null,
      valid: false,
      emailRules: [
        (v) => !!v || 'Пожалуйста введите email',
        (v) => /^([a-z0-9_-]+\.)*[a-z0-9_-]+@[a-z0-9_-]+(\.[a-z0-9_-]+)*\.[a-z]{2,6}$/.test(v) || "Неправильный email",
      ],
      passwordRules: [
        (v) => !!v || 'Пожалуйста введите пароль',
        (v) => (v && v.length >= 6) || 'Пароль слишком короткий - минимум 6 символов'
      ]
		};
	},
  computed: {
    error() {
      return this.$store.getters.getError
    },
    processing() {
      return this.$store.getters.getProcessing
    },
    isUserAuthenticated() {
      return this.$store.getters.isUserAuthenticated
    }
  },
  watch: {
    isUserAuthenticated(val) {
      if(val === true) {
        this.$router.push('/')
      }
    }
  },
	methods: {
		signup() {
			this.$store.dispatch("SIGNUP", {
				email: this.email,
				password: this.password
			});
		}
	}
};
</script>

<style>
</style>
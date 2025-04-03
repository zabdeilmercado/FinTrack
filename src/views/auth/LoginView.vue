<template>
  <v-container fluid class="fill-height bg-surface">
    <v-row justify="center" align="center" class="fill-height">
      <v-col cols="12" sm="8" md="6" lg="4">
        <v-card class="mx-auto pa-4" elevation="8" rounded="lg">
          <v-card-title class="text-center text-h4 font-weight-bold">Welcome back</v-card-title>
          <v-card-subtitle class="text-center pb-4">Sign in to your account to continue</v-card-subtitle>
          
          <v-form @submit.prevent="handleSubmit">
            <v-card-text>
              <v-text-field
                v-model="email"
                label="Email"
                type="email"
                placeholder="name@example.com"
                variant="outlined"
                prepend-inner-icon="mdi-email-outline"
                required
                :rules="[rules.required, rules.email]"
              ></v-text-field>
              
              <div class="d-flex justify-space-between align-center mb-2">
                <div class="text-body-1">Password</div>
                <v-btn
                  variant="text"
                  density="compact"
                  class="text-caption text-medium-emphasis"
                  to="/forgot-password"
                >
                  Forgot password?
                </v-btn>
              </div>
              
              <v-text-field
                v-model="password"
                :type="showPassword ? 'text' : 'password'"
                variant="outlined"
                prepend-inner-icon="mdi-lock-outline"
                required
                :rules="[rules.required]"
                :append-inner-icon="showPassword ? 'mdi-eye-off' : 'mdi-eye'"
                @click:append-inner="showPassword = !showPassword"
              ></v-text-field>
              
              <v-checkbox
                v-model="rememberMe"
                label="Remember me"
                color="primary"
                hide-details
              ></v-checkbox>
            </v-card-text>
            
            <v-card-actions class="flex-column">
              <v-btn
                type="submit"
                color="primary"
                block
                size="large"
                class="mb-4"
                :loading="loading"
              >
                Sign in
              </v-btn>
              
              <div class="text-center text-body-2">
                Don't have an account?
                <v-btn
                  variant="text"
                  color="primary"
                  class="font-weight-medium px-1"
                  to="/register"
                >
                  Create one
                </v-btn>
              </div>
            </v-card-actions>
          </v-form>
        </v-card>
      </v-col>
    </v-row>

    <!-- Theme toggle button -->
    <v-btn
      icon
      variant="text"
      class="theme-toggle"
      @click="toggleTheme"
    >
      <v-icon>{{ isDarkTheme ? 'mdi-weather-sunny' : 'mdi-weather-night' }}</v-icon>
    </v-btn>
  </v-container>
</template>

<script setup>
import { ref, computed } from 'vue';
import { useRouter } from 'vue-router';
import { useTheme } from 'vuetify';

const router = useRouter();
const theme = useTheme();

const isDarkTheme = computed(() => theme.global.current.value.dark);
const email = ref('');
const password = ref('');
const showPassword = ref(false);
const rememberMe = ref(false);
const loading = ref(false);

const rules = {
  required: value => !!value || 'Required.',
  email: value => {
    const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return pattern.test(value) || 'Invalid e-mail.';
  },
};

const toggleTheme = () => {
  theme.global.name.value = isDarkTheme.value ? 'light' : 'dark';
  localStorage.setItem('theme', theme.global.name.value);
};

const handleSubmit = async () => {
  loading.value = true;
  
  try {
    // Here you would handle the actual login logic
    console.log('Login attempt with:', { 
      email: email.value, 
      password: password.value, 
      rememberMe: rememberMe.value 
    });
    
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1000));
    
    // For demo purposes, we'll just redirect to a dashboard
    router.push('/dashboard');
  } catch (error) {
    console.error('Login error:', error);
  } finally {
    loading.value = false;
  }
};
</script>

<style scoped>
.theme-toggle {
  position: absolute;
  top: 16px;
  right: 16px;
  z-index: 10;
}
</style>
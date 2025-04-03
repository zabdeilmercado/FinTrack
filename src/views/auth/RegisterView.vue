<template>
    <v-container fluid class="fill-height bg-surface">
      <v-row justify="center" align="center" class="fill-height">
        <v-col cols="12" sm="8" md="6" lg="4">
          <v-card class="mx-auto pa-4" elevation="8" rounded="lg">
            <v-card-title class="text-center text-h4 font-weight-bold">Create an account</v-card-title>
            <v-card-subtitle class="text-center pb-4">Enter your information to get started</v-card-subtitle>
            
            <v-form @submit.prevent="handleSubmit" ref="form">
              <v-card-text>
                <v-text-field
                  v-model="name"
                  label="Full Name"
                  placeholder="John Doe"
                  variant="outlined"
                  prepend-inner-icon="mdi-account-outline"
                  required
                  :rules="[rules.required]"
                ></v-text-field>
                
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
                
                <v-text-field
                  v-model="password"
                  label="Password"
                  :type="showPassword ? 'text' : 'password'"
                  variant="outlined"
                  prepend-inner-icon="mdi-lock-outline"
                  required
                  :rules="[rules.required, rules.password]"
                  :append-inner-icon="showPassword ? 'mdi-eye-off' : 'mdi-eye'"
                  @click:append-inner="showPassword = !showPassword"
                  hint="Password must be at least 8 characters"
                ></v-text-field>
                
                <v-text-field
                  v-model="confirmPassword"
                  label="Confirm Password"
                  :type="showConfirmPassword ? 'text' : 'password'"
                  variant="outlined"
                  prepend-inner-icon="mdi-lock-check-outline"
                  required
                  :rules="[rules.required, passwordMatchRule]"
                  :append-inner-icon="showConfirmPassword ? 'mdi-eye-off' : 'mdi-eye'"
                  @click:append-inner="showConfirmPassword = !showConfirmPassword"
                ></v-text-field>
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
                  Create account
                </v-btn>
                
                <div class="text-center text-body-2">
                  Already have an account?
                  <v-btn
                    variant="text"
                    color="primary"
                    class="font-weight-medium px-1"
                    to="/login"
                  >
                    Sign in
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
  const form = ref(null);
  
  const isDarkTheme = computed(() => theme.global.current.value.dark);
  const name = ref('');
  const email = ref('');
  const password = ref('');
  const confirmPassword = ref('');
  const showPassword = ref(false);
  const showConfirmPassword = ref(false);
  const loading = ref(false);
  
  const rules = {
    required: value => !!value || 'Required.',
    email: value => {
      const pattern = /^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
      return pattern.test(value) || 'Invalid e-mail.';
    },
    password: value => {
      const hasMinLength = value && value.length >= 8;
      return hasMinLength || 'Password must be at least 8 characters.';
    },
  };
  
  const passwordMatchRule = value => value === password.value || 'Passwords do not match.';
  
  const toggleTheme = () => {
    theme.global.name.value = isDarkTheme.value ? 'light' : 'dark';
    localStorage.setItem('theme', theme.global.name.value);
  };
  
  const handleSubmit = async () => {
    const { valid } = await form.value.validate();
    
    if (!valid) return;
    
    loading.value = true;
    
    try {
      // Here you would handle the actual registration logic
      console.log('Registration attempt with:', { 
        name: name.value, 
        email: email.value, 
        password: password.value 
      });
      
      // Simulate API call
      await new Promise(resolve => setTimeout(resolve, 1000));
      
      // For demo purposes, we'll just redirect to a dashboard
      router.push('/dashboard');
    } catch (error) {
      console.error('Registration error:', error);
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
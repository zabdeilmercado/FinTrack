<template>
  <div class="login-container">
    <div class="login-card">
      <!-- Logo and Header -->
      <div class="logo-section">
        <div class="logo">
          <img src="../assets/logo.svg" alt="FinTrack Logo" />
        </div>
        <h1>FinTrack</h1>
        <p>Your personal finance tracker</p>
      </div>

      <!-- Login/Register Tabs -->
      <div class="tabs">
        <button 
          :class="['tab-btn', { active: activeTab === 'login' }]" 
          @click="activeTab = 'login'"
        >
          Login
        </button>
        <button 
          :class="['tab-btn', { active: activeTab === 'register' }]" 
          @click="activeTab = 'register'"
        >
          Register
        </button>
      </div>

      <!-- Login Form -->
      <div v-if="activeTab === 'login'" class="form-container">
        <form @submit.prevent="handleLogin">
          <div class="form-group">
            <label for="email">Email</label>
            <div class="input-with-icon">
              <i class="icon-email"></i>
              <input 
                type="email" 
                id="email" 
                v-model="loginEmail" 
                placeholder="name@example.com"
                required
              />
            </div>
            <span v-if="loginErrors.email" class="error-message">{{ loginErrors.email }}</span>
          </div>

          <div class="form-group">
            <div class="label-row">
              <label for="password">Password</label>
              <a href="#" class="forgot-link" @click.prevent="showForgotPassword = true">Forgot password?</a>
            </div>
            <div class="input-with-icon">
              <i class="icon-lock"></i>
              <input 
                :type="showPassword ? 'text' : 'password'" 
                id="password" 
                v-model="loginPassword" 
                placeholder="••••••••"
                required
              />
              <button 
                type="button" 
                class="toggle-password" 
                @click="showPassword = !showPassword"
              >
                <i :class="showPassword ? 'icon-eye-off' : 'icon-eye'"></i>
              </button>
            </div>
            <span v-if="loginErrors.password" class="error-message">{{ loginErrors.password }}</span>
          </div>

          <div class="remember-me">
            <label class="checkbox-container">
              <input type="checkbox" v-model="rememberMe" />
              <span class="checkmark"></span>
              Remember me
            </label>
          </div>

          <button 
            type="submit" 
            class="submit-btn" 
            :disabled="isLoading"
          >
            <span v-if="isLoading" class="loader"></span>
            <span v-else>Login</span>
          </button>
        </form>
      </div>

      <!-- Register Form -->
      <div v-if="activeTab === 'register'" class="form-container">
        <form @submit.prevent="handleRegister">
          <div class="form-group">
            <label for="name">Full Name</label>
            <div class="input-with-icon">
              <i class="icon-user"></i>
              <input 
                type="text" 
                id="name" 
                v-model="registerName" 
                placeholder="John Doe"
                required
              />
            </div>
            <span v-if="registerErrors.name" class="error-message">{{ registerErrors.name }}</span>
          </div>

          <div class="form-group">
            <label for="register-email">Email</label>
            <div class="input-with-icon">
              <i class="icon-email"></i>
              <input 
                type="email" 
                id="register-email" 
                v-model="registerEmail" 
                placeholder="name@example.com"
                required
              />
            </div>
            <span v-if="registerErrors.email" class="error-message">{{ registerErrors.email }}</span>
          </div>

          <div class="form-group">
            <label for="register-password">Password</label>
            <div class="input-with-icon">
              <i class="icon-lock"></i>
              <input 
                :type="showPassword ? 'text' : 'password'" 
                id="register-password" 
                v-model="registerPassword" 
                placeholder="••••••••"
                required
              />
              <button 
                type="button" 
                class="toggle-password" 
                @click="showPassword = !showPassword"
              >
                <i :class="showPassword ? 'icon-eye-off' : 'icon-eye'"></i>
              </button>
            </div>
            <span v-if="registerErrors.password" class="error-message">{{ registerErrors.password }}</span>
          </div>

          <div class="form-group">
            <label for="confirm-password">Confirm Password</label>
            <div class="input-with-icon">
              <i class="icon-lock"></i>
              <input 
                :type="showPassword ? 'text' : 'password'" 
                id="confirm-password" 
                v-model="confirmPassword" 
                placeholder="••••••••"
                required
              />
            </div>
            <span v-if="registerErrors.confirmPassword" class="error-message">{{ registerErrors.confirmPassword }}</span>
          </div>

          <button 
            type="submit" 
            class="submit-btn" 
            :disabled="isLoading"
          >
            <span v-if="isLoading" class="loader"></span>
            <span v-else>Create Account</span>
          </button>
        </form>
      </div>

      <!-- Social Login Options -->
      <div class="social-login">
        <p>Or continue with</p>
        <div class="social-buttons">
          <button class="social-btn google">
            <i class="icon-google"></i>
          </button>
          <button class="social-btn facebook">
            <i class="icon-facebook"></i>
          </button>
          <button class="social-btn apple">
            <i class="icon-apple"></i>
          </button>
        </div>
      </div>

      <!-- Terms and Conditions -->
      <div class="terms">
        <p>
          By continuing, you agree to our 
          <a href="#">Terms of Service</a> and 
          <a href="#">Privacy Policy</a>
        </p>
      </div>
    </div>

    <!-- Forgot Password Modal -->
    <div v-if="showForgotPassword" class="modal-overlay">
      <div class="modal">
        <div class="modal-header">
          <h2>Reset Password</h2>
          <button class="close-btn" @click="showForgotPassword = false">×</button>
        </div>
        <div class="modal-body">
          <p>Enter your email address and we'll send you a link to reset your password.</p>
          <div class="form-group">
            <label for="forgot-email">Email</label>
            <div class="input-with-icon">
              <i class="icon-email"></i>
              <input 
                type="email" 
                id="forgot-email" 
                v-model="forgotEmail" 
                placeholder="name@example.com"
                required
              />
            </div>
            <span v-if="forgotErrors.email" class="error-message">{{ forgotErrors.email }}</span>
          </div>
        </div>
        <div class="modal-footer">
          <button class="cancel-btn" @click="showForgotPassword = false">Cancel</button>
          <button 
            class="submit-btn" 
            @click="handleForgotPassword"
            :disabled="isLoading"
          >
            <span v-if="isLoading" class="loader"></span>
            <span v-else>Send Reset Link</span>
          </button>
        </div>
      </div>
    </div>

    <!-- Toast Notification -->
    <div v-if="showToast" :class="['toast', toastType]">
      <p>{{ toastMessage }}</p>
      <button class="close-toast" @click="showToast = false">×</button>
    </div>
  </div>
</template>

<script setup>
import { ref, reactive } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

// Form state
const activeTab = ref('login');
const showPassword = ref(false);
const isLoading = ref(false);
const rememberMe = ref(false);
const showForgotPassword = ref(false);

// Login form
const loginEmail = ref('');
const loginPassword = ref('');
const loginErrors = reactive({
  email: '',
  password: ''
});

// Register form
const registerName = ref('');
const registerEmail = ref('');
const registerPassword = ref('');
const confirmPassword = ref('');
const registerErrors = reactive({
  name: '',
  email: '',
  password: '',
  confirmPassword: ''
});

// Forgot password
const forgotEmail = ref('');
const forgotErrors = reactive({
  email: ''
});

// Toast notification
const showToast = ref(false);
const toastMessage = ref('');
const toastType = ref('success');

// Validation functions
const validateEmail = (email) => {
  const re = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return re.test(email);
};

const validatePassword = (password) => {
  return password.length >= 8;
};

const validateName = (name) => {
  return name.length >= 3;
};

// Form handlers
const handleLogin = async () => {
  // Reset errors
  loginErrors.email = '';
  loginErrors.password = '';
  
  // Validate form
  let isValid = true;
  
  if (!loginEmail.value) {
    loginErrors.email = 'Email is required';
    isValid = false;
  } else if (!validateEmail(loginEmail.value)) {
    loginErrors.email = 'Please enter a valid email';
    isValid = false;
  }
  
  if (!loginPassword.value) {
    loginErrors.password = 'Password is required';
    isValid = false;
  } else if (!validatePassword(loginPassword.value)) {
    loginErrors.password = 'Password must be at least 8 characters';
    isValid = false;
  }
  
  if (!isValid) return;
  
  isLoading.value = true;
  
  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1500));
    
    // Store token (in a real app, this would come from your API)
    localStorage.setItem('userToken', 'demo-token');
    
    // Show success message
    displayToast('Login successful!', 'success');
    
    // Redirect to dashboard
    router.push('/dashboard');
  } catch (error) {
    displayToast('Login failed. Please check your credentials.', 'error');
  } finally {
    isLoading.value = false;
  }
};

const handleRegister = async () => {
  // Reset errors
  registerErrors.name = '';
  registerErrors.email = '';
  registerErrors.password = '';
  registerErrors.confirmPassword = '';
  
  // Validate form
  let isValid = true;
  
  if (!registerName.value) {
    registerErrors.name = 'Name is required';
    isValid = false;
  } else if (!validateName(registerName.value)) {
    registerErrors.name = 'Name must be at least 3 characters';
    isValid = false;
  }
  
  if (!registerEmail.value) {
    registerErrors.email = 'Email is required';
    isValid = false;
  } else if (!validateEmail(registerEmail.value)) {
    registerErrors.email = 'Please enter a valid email';
    isValid = false;
  }
  
  if (!registerPassword.value) {
    registerErrors.password = 'Password is required';
    isValid = false;
  } else if (!validatePassword(registerPassword.value)) {
    registerErrors.password = 'Password must be at least 8 characters';
    isValid = false;
  }
  
  if (registerPassword.value !== confirmPassword.value) {
    registerErrors.confirmPassword = 'Passwords do not match';
    isValid = false;
  }
  
  if (!isValid) return;
  
  isLoading.value = true;
  
  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1500));
    
    // Store token (in a real app, this would come from your API)
    localStorage.setItem('userToken', 'demo-token');
    
    // Show success message
    displayToast('Registration successful!', 'success');
    
    // Redirect to dashboard
    router.push('/dashboard');
  } catch (error) {
    displayToast('Registration failed. Please try again.', 'error');
  } finally {
    isLoading.value = false;
  }
};

const handleForgotPassword = async () => {
  // Reset errors
  forgotErrors.email = '';
  
  // Validate form
  let isValid = true;
  
  if (!forgotEmail.value) {
    forgotErrors.email = 'Email is required';
    isValid = false;
  } else if (!validateEmail(forgotEmail.value)) {
    forgotErrors.email = 'Please enter a valid email';
    isValid = false;
  }
  
  if (!isValid) return;
  
  isLoading.value = true;
  
  try {
    // Simulate API call
    await new Promise(resolve => setTimeout(resolve, 1500));
    
    // Show success message
    displayToast('Password reset link sent to your email!', 'success');
    
    // Close modal
    showForgotPassword.value = false;
  } catch (error) {
    displayToast('Failed to send reset link. Please try again.', 'error');
  } finally {
    isLoading.value = false;
  }
};

const displayToast = (message, type = 'success') => {
  toastMessage.value = message;
  toastType.value = type;
  showToast.value = true;
  
  // Auto-hide toast after 3 seconds
  setTimeout(() => {
    showToast.value = false;
  }, 3000);
};
</script>

<style scoped>
/* Login Page Styles */
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  padding: 20px;
  background-color: #f5f7fa;
}

.login-card {
  width: 100%;
  max-width: 450px;
  background-color: white;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  padding: 30px;
}

.logo-section {
  text-align: center;
  margin-bottom: 24px;
}

.logo {
  width: 80px;
  height: 80px;
  background-color: var(--primary);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto 16px;
}

.logo img {
  width: 40px;
  height: 40px;
}

.logo-section h1 {
  font-size: 24px;
  font-weight: 700;
  color: var(--dark);
  margin-bottom: 4px;
}

.logo-section p {
  font-size: 14px;
  color: var(--gray);
}

/* Tabs */
.tabs {
  display: flex;
  margin-bottom: 24px;
  border-bottom: 1px solid #e0e0e0;
}

.tab-btn {
  flex: 1;
  background: none;
  border: none;
  padding: 12px;
  font-size: 16px;
  font-weight: 500;
  color: var(--gray);
  cursor: pointer;
  transition: all 0.3s ease;
  position: relative;
}

.tab-btn.active {
  color: var(--primary);
}

.tab-btn.active::after {
  content: '';
  position: absolute;
  bottom: -1px;
  left: 0;
  width: 100%;
  height: 2px;
  background-color: var(--primary);
}

/* Form Styles */
.form-container {
  margin-bottom: 24px;
}

.form-group {
  margin-bottom: 16px;
}

label {
  display: block;
  font-size: 14px;
  font-weight: 500;
  margin-bottom: 6px;
  color: var(--dark);
}

.label-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.forgot-link {
  font-size: 12px;
  color: var(--primary);
  text-decoration: none;
}

.input-with-icon {
  position: relative;
}

.input-with-icon input {
  width: 100%;
  padding: 12px 12px 12px 40px;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  font-size: 14px;
  transition: border-color 0.3s ease;
}

.input-with-icon input:focus {
  outline: none;
  border-color: var(--primary);
  box-shadow: 0 0 0 2px rgba(24, 103, 192, 0.1);
}

.input-with-icon i {
  position: absolute;
  left: 12px;
  top: 50%;
  transform: translateY(-50%);
  color: var(--gray);
}

.toggle-password {
  position: absolute;
  right: 12px;
  top: 50%;
  transform: translateY(-50%);
  background: none;
  border: none;
  color: var(--gray);
  cursor: pointer;
}

.error-message {
  display: block;
  color: var(--error);
  font-size: 12px;
  margin-top: 4px;
}

/* Checkbox */
.remember-me {
  margin-bottom: 20px;
}

.checkbox-container {
  display: flex;
  align-items: center;
  font-size: 14px;
  color: var(--dark);
  cursor: pointer;
}

.checkbox-container input {
  margin-right: 8px;
}

/* Submit Button */
.submit-btn {
  width: 100%;
  padding: 12px;
  background-color: var(--primary);
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 16px;
  font-weight: 500;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.submit-btn:hover {
  background-color: var(--primary-dark);
}

.submit-btn:disabled {
  background-color: var(--gray);
  cursor: not-allowed;
}

/* Social Login */
.social-login {
  text-align: center;
  margin-bottom: 24px;
}

.social-login p {
  font-size: 14px;
  color: var(--gray);
  margin-bottom: 16px;
  position: relative;
}

.social-login p::before,
.social-login p::after {
  content: '';
  position: absolute;
  top: 50%;
  width: 30%;
  height: 1px;
  background-color: #e0e0e0;
}

.social-login p::before {
  left: 0;
}

.social-login p::after {
  right: 0;
}

.social-buttons {
  display: flex;
  justify-content: center;
  gap: 16px;
}

.social-btn {
  width: 48px;
  height: 48px;
  border-radius: 50%;
  border: 1px solid #e0e0e0;
  background-color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.3s ease;
}

.social-btn:hover {
  background-color: #f5f5f5;
}

/* Terms */
.terms {
  text-align: center;
  font-size: 12px;
  color: var(--gray);
}

.terms a {
  color: var(--primary);
  text-decoration: none;
}

/* Modal */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
}

.modal {
  background-color: white;
  border-radius: 12px;
  width: 90%;
  max-width: 500px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 24px;
  border-bottom: 1px solid #e0e0e0;
}

.modal-header h2 {
  font-size: 18px;
  font-weight: 600;
}

.close-btn {
  background: none;
  border: none;
  font-size: 24px;
  cursor: pointer;
  color: var(--gray);
}

.modal-body {
  padding: 24px;
}

.modal-body p {
  margin-bottom: 16px;
  font-size: 14px;
  color: var(--gray);
}

.modal-footer {
  padding: 16px 24px;
  border-top: 1px solid #e0e0e0;
  display: flex;
  justify-content: flex-end;
  gap: 12px;
}

.cancel-btn {
  padding: 8px 16px;
  background-color: white;
  border: 1px solid #e0e0e0;
  border-radius: 6px;
  font-size: 14px;
  cursor: pointer;
}

/* Toast */
.toast {
  position: fixed;
  bottom: 24px;
  right: 24px;
  padding: 12px 24px;
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  min-width: 300px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  z-index: 1000;
}

.toast.success {
  background-color: #4CAF50;
  color: white;
}

.toast.error {
  background-color: #FF5252;
  color: white;
}

.close-toast {
  background: none;
  border: none;
  color: white;
  font-size: 18px;
  cursor: pointer;
  margin-left: 12px;
}

/* Loader */
.loader {
  display: inline-block;
  width: 20px;
  height: 20px;
  border: 2px solid rgba(255, 255, 255, 0.3);
  border-radius: 50%;
  border-top-color: white;
  animation: spin 1s ease-in-out infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

/* Icons (using pseudo-elements for demo) */
.icon-email::before {
  content: "✉️";
}

.icon-lock::before {
  content: "🔒";
}

.icon-eye::before {
  content: "👁️";
}

.icon-eye-off::before {
  content: "🙈";
}

.icon-user::before {
  content: "👤";
}

.icon-google::before {
  content: "G";
}

.icon-facebook::before {
  content: "f";
}

.icon-apple::before {
  content: "🍎";
}

/* Responsive adjustments */
@media (max-width: 480px) {
  .login-card {
    padding: 20px;
  }
  
  .logo {
    width: 60px;
    height: 60px;
  }
  
  .logo img {
    width: 30px;
    height: 30px;
  }
  
  .toast {
    left: 24px;
    right: 24px;
    min-width: auto;
  }
}
</style>
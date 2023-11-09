<template>
  <div v-if="!showModal">
    <form @submit.prevent="submitForm" >
      <div class="h1">
        <p>Регистрация</p>
      </div>
      <p class="h2">Заполните Ваши данные</p>
      <div class="information">
        <input type="text" v-model="username" placeholder="Имя">
        <input type="email" v-model="email" placeholder="Email">
        <div style="width: 302px;"></div>
        <select v-model="role">
          <option value="">Должность</option>
          <option value="1">Менеджер</option>
          <option value="2">Разработчик</option>
          <option value="3">Дизайнер</option>
        </select>
        <input type="password" v-model="password" placeholder="Пароль">
        <input type="password" v-model="passwordRepeat" placeholder="Повторите пароль">
      </div>
      <div class="line_bottom"><p></p></div>
      <div class="bottom_inf">
        <div>
          <label class="checkbox style-e" for="public">
            <input type="checkbox" id="public" v-model="public" checked />
            <div class="checkbox__checkmark"></div>
            <div class="checkbox__body">
              <p class="public"> Хотите чтобы Ваш профиль видели другие участники платформы? </p>
              <p class="public_2">Включает профиль для просмотра другими пользователями по ссылке</p>
            </div>
          </label>
          <label class="checkbox style-c" for="consent">
            <input type="checkbox" id="consent" v-model="consent" checked />
            <div class="checkbox__checkmark"></div>
            <div class="checkbox__body">Регистрируясь, Вы соглашаетесь с <font color="#3586FF">политикой конфиденциальности <br> </font>  и обработкой <font color="#3586FF">персональных данных </font> </div>
          </label>
        </div>
        <div class="submit_btn">
          <button class="submit" type="submit">Зарегистрироваться</button>
        </div>
       
      </div>

      <div class="error" v-if="error">{{ error }}</div>
    
    </form>
    
  </div>
  <!-- style="display: none; -->
  <div class="main-container" v-if="showModal" >
	<div class="check-container">
		<div class="check-background">
			<svg viewBox="0 0 65 51" fill="none" xmlns="http://www.w3.org/2000/svg">
				<path d="M7 25L27.3077 44L58.5 7" stroke="white" stroke-width="13" stroke-linecap="round" stroke-linejoin="round" />
			</svg>
		</div>
		<div class="check-shadow"></div>
	</div>
    <p>Регистрация успешно завершена</p>
</div>
</template>


<style src="./main.css"></style>





<script>
export default {
  data() {
    return {
      public: true,
      username: '',
      email: '',
      role: '',
      password: '',
      passwordRepeat: '',
      consent: true,
      error: '',
      
      showModal: false,
    };
  },
  methods: {
    submitForm() {
      const data = {
        public: this.public,
        username: this.username,
        role: this.role,
        email: this.email,
        password: this.password,
        passwordRepeat: this.passwordRepeat,
      };


      if (!data.username || !data.role || !data.email || !data.password || !data.passwordRepeat) {
        this.error = 'Пожалуйста, заполните все поля';
      
                document.querySelectorAll('select').forEach(select => {
                if (!select.value) {
                  select.style.borderColor = 'red';
                } 
                else {
                  select.style.borderColor = 'green';
                }
            });
     
          
        document.querySelectorAll('input').forEach(input => {
                if (!input.value) {
                    input.style.borderColor = 'red';
                } 
                else {
                    input.style.borderColor = 'green';
                }
            });
        return;
      }
      if (this.password !== this.passwordRepeat) {
        this.error = 'Пароли не совпадают';
        document.querySelectorAll('input[type="password"]').forEach(input => {
                input.style.borderColor = 'red';
            });
            return;
      }
      if (this.password.length < 7) {
        this.error = 'Пароль должен содержать не менее 7 символов';
        return;
      }
      this.error = '';

      if (!this.consent) {
        this.error = 'Вы должны дать согласие на обработку персональных данных';
        return;
      }

      // Здесь можете выполнить запрос к серверу для регистрации пользователя
      // Пример запроса:
    
     
      fetch('https://jsonplaceholder.typicode.com/posts', {
        method: 'POST',
        body: JSON.stringify(data),
        headers: {
          'Content-Type': 'application/json',
        },
      })
      .then(response => response.json())
  .then(() => {
    this.showModal = true;
  })
  .catch(error => {
    console.error('Ошибка при регистрации', error);
  });
      
    },
  },
};
</script>




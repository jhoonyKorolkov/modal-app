<template>
  <modal title="Регистрация" @close="$emit('close')">
    <form slot="body" @submit.prevent="sendForm" novalidate>
      <div class="form__item" v-for="(item, key) in form" :key="item.id">
        <label class="form__item-name" :for="item.id">{{ item.label }}</label>
        <input
          class="form__item-field"
          :type="item.type"
          v-model="item.value"
          :id="item.id"
          :class="{ error: item.error }"
          @focus="clearError(key)"
        />
        <div v-if="item.error" class="error">{{ item.error }}</div>
      </div>
      <!-- button -->
      <button class="btn btnPrimary">Submit!</button>
    </form>
    <footer slot="footer">
      Уже есть аккаунт? <button @click="$emit('change-modal')">Войти</button>
    </footer>
  </modal>
</template>

<script>
import Modal from './UI/Modal.vue';
export default {
  components: { Modal },
  name: 'reg',
  data() {
    return {
      form: {
        email: {
          value: '',
          type: 'email',
          label: 'E-mail',
          pattern: /^([\w.*-]+@([\w-]+\.)+[\w-]{2,4})?$/,
          error: '',
          id: 1,
        },
        password: {
          value: '',
          type: 'password',
          label: 'Пароль',
          error: '',
          id: 2,
          pattern: /^.{6,}$/,
        },
        password2: {
          value: '',
          type: 'password',
          label: 'Повторите пароль',
          error: '',
          id: 3,
          pattern: /^.{6,}$/,
        },
      },
    };
  },

  computed: {
    formIsValid() {
      return Object.values(this.form).every((el) => el.error === '');
    },
  },

  methods: {
    sendForm() {
      this.validation();

      if (!this.formIsValid) return;

      const data = {
        email: this.form.email.value,
        password: this.form.password.value,
      };
      console.log(data);
    },

    validation() {
      const errorMessages = {
        emptyError: 'Необходимо заполнить поле',
        email: 'Не верно указан почтовый адрес',
        password: 'Пароль должен быть не меньше 6 символов',
        password2: 'Пароль должен быть не меньше 6 символов',
        repeat: 'Пароли не совпадают',
      };

      Object.entries(this.form).forEach(([key, field]) => {
        if (!field.value) {
          this.form[key].error = errorMessages.emptyError;
          return;
        }

        if (!field.pattern.test(field.value)) {
          this.form[key].error = errorMessages[key];
          return;
        }

        if (this.form.password.value !== this.form.password2.value) {
          this.form.password2.error = errorMessages.repeat;
        }
      });
    },

    clearError(key) {
      this.form[key].error = '';
    },
  },
};
</script>

<style lang="scss"></style>

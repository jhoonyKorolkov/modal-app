<template>
  <div class="wrapper">
    <div class="wrapper-content">
      <section>
        <div class="container">
          <!-- first modal -->
          <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Show first modal</button>
          <modals title="First modal" v-show="modalFirst" @close="modalFirst = false">
            <!-- body -->
            <div slot="body">
              <p>Text Text Text Text Text Text Text</p>
              <button class="btn btnPrimary" @click="modalFirst = !modalFirst">Well Done!</button>
            </div>
          </modals>

          <!-- second modal -->
          <button class="btn btnPrimary" @click="modalSecond.show = !modalSecond.show">
            Show modal with form
          </button>
          <modals
            title="Modal with form"
            v-show="modalSecond.show"
            @close="modalSecond.show = false"
          >
            <!-- body -->
            <div slot="body">
              <form @submit.prevent="submitSecondForm">
                <label>Name:</label>
                <input type="text" required v-model="modalSecond.name" />
                <label>Email:</label>
                <input type="email" required v-model="modalSecond.email" />
                <button class="btn btnPrimary">Submit!</button>
              </form>
            </div>
          </modals>

          <!-- modal with validate -->
          <button class="btn btnPrimary" @click="modalValidate = !modalValidate">
            Show modal with form + validate
          </button>
          <modalValidate v-show="modalValidate" @close="modalValidate = false" />
        </div>

        <!--ДЗ-->
        <div class="container">
          <button class="btn btnPrimary" @click="openModal('modal-auth')">auth</button>
          <transition name="modal">
            <!--Вот так работает анимация с v-if-->
            <auth
              v-if="modalActive == 'modal-auth'"
              @close="modalActive = null"
              @change-modal="modalActive = 'modal-reg'"
            ></auth>
          </transition>
          <button class="btn btnPrimary" @click="openModal('modal-reg')">reg</button>
          <transition name="modal">
            <reg
              v-if="modalActive == 'modal-reg'"
              @close="modalActive = null"
              @change-modal="modalActive = 'modal-auth'"
            ></reg>
          </transition>
        </div>
        <!--ДЗ-->
      </section>
    </div>
  </div>
</template>

<script>
import modals from '@/components/UI/Modal.vue';
import modalValidate from '@/components/ModalValidate.vue';
import Reg from './components/Reg.vue';
import Auth from './components/Auth.vue';

export default {
  components: {
    modals,
    modalValidate,
    Reg,
    Auth,
  },
  data() {
    return {
      modalFirst: false,
      modalSecond: {
        show: false,
        name: '',
        email: '',
      },
      modalValidate: false,
      modalActive: null,
    };
  },
  methods: {
    openModal(modalName) {
      if (this.modalActive === modalName) return (this.modalActive = null);
      this.modalActive = modalName;
    },
    submitSecondForm() {
      console.log({
        name: this.modalSecond.name,
        email: this.modalSecond.email,
      });
      this.modalSecond.name = '';
      this.modalSecond.email = '';
      this.modalSecond.show = false;
    },
  },
};
</script>

<style lang="scss">
// Animation
.modal-enter {
  opacity: 0;
}
.modal-leave-active {
  opacity: 0;
}
.modal-enter .modal-content,
.modal-leave-active .modal-content {
  transform: scale(1.2);
}
</style>

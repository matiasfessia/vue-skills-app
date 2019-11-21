<template>
  <div class="container">
    <div class="holder">
      <form @submit.prevent="addSkill">
        <ValidationObserver ref="observer" v-slot="{ invalid }"  @submit.prevent="submit()">
          <ValidationProvider name="skill" rules="min:5">
            <div slot-scope="{ errors }">
              <input v-model="skill">
              <transition name="alert-in">
                <p v-if="errors[0]">{{ errors[0] }}</p>
              </transition>
            </div>
          </ValidationProvider>
        </ValidationObserver>
      </form>
      <ul>
        <transition-group name="list" enter-active-class="animated bounceInUp" leave-active-class="animated bounceOutDown">
          <li v-for="(data, index) in skills" :key='index'>
            {{data.name}}
            <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
          </li>
        </transition-group>
      </ul>
      <p>These are the skills that you possess.</p>
    </div>
  </div>
</template>

<script>
import { min } from 'vee-validate/dist/rules';
import { extend } from 'vee-validate';

// Add the required rule
extend('min', {
  ...min,
  message: 'The skill must have at least five charar...'
});

export default {
  name: 'Skills',
  data() {
    return {
      skill: '',
      skills: [
        { name: 'Vuejs' },
        { name: 'React' },
        { name: 'Angular' }
      ],
    }
  },
  methods: {
    async addSkill() {
      const isValid = await this.$refs.observer.validate();
      if (isValid) {
        this.skills = [...this.skills, { name: this.skill }];
        this.skill = '';
      } else {
        alert('The skill introduced is not valid');
      }
    },
    remove(index) {
      this.skills.splice(index, 1);
    }
  },
  props: {
    msg: String
  }
}
</script>

<style scoped>
  @import "https://cdnjs.cloudflare.com/ajax/libs/animate.css/3.7.2/animate.min.css";

  .holder {
    background: #fff;
  }

  ul {
    margin: 0;
    padding: 0;
    list-style-type: none;
  }
  
  ul li {
    padding: 20px;
    font-size: 1.3em;
    background-color: #E0EDF4;
    border-left: 5px solid #3EB3F6;
    margin-bottom: 2px;
    color: #3E5252;
  }

  p {
    text-align:center;
    padding: 30px 0;
    color: gray;
  }

  .container {
    box-shadow: 0px 0px 40px lightgray;
  }

  input {
    width: calc(100% - 40px);
    border: 0;
    padding: 20px;
    font-size: 1.3em;
    background-color: #332233;
    color: #687F7F;
  }

  .alert-in-enter-active {
    animation: bounce-in .5s;
  }

  .alert-in-leave-active {
    animation: bounce-in .5s reverse;
  }

  @keyframes bounce-in {
    0% {
      transform: scale(0);
    }
    50% {
      transform: scale(1.5);
    }
    100% {
      transform: scale(1);
    }
  }

  i {
    float: right;
    cursor: pointer;
  }
</style>

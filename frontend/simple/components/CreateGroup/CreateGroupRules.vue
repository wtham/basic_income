<template>
  <div id="rulesStep" class="has-text-centered">
    <h1 class="title is-2"><i18n>Voting Rules</i18n></h1>
    <p class="content"><i18n>What percentage approval is necessary to adjust the group rules?</i18n></p>
    <div class="columns">
      <div class="column">
        <div class="rule">
          <p class="percent">{{ group.changePercentage }}%</p>
          <circle-slider
            class="circle-slider"
            :value="group.changePercentage"
            @input="value => update('changePercentage', value)"
            :side="160"
            :circleWidth="1"
            :progressWidth="2"
            :knobRadius="5"
            :progressColor="changeColor"
            :knobColor="changeColor"
            circleColor="#69707a"
          >
          </circle-slider>
          <p class="title is-6"><i18n>Change Rules</i18n></p>
        </div>
      </div>
      <div class="column">
        <div class="rule">
          <p class="percent">{{ group.memberApprovalPercentage }}%</p>
          <circle-slider
            class="circle-slider"
            :value="group.memberApprovalPercentage"
            @input="value => update('memberApprovalPercentage', value)"
            :side="160"
            :circleWidth="1"
            :progressWidth="2"
            :knobRadius="5"
            :progressColor="approveColor"
            :knobColor="approveColor"
            circleColor="#69707a"
          >
          </circle-slider>
          <p class="title is-6"><i18n>Add Member</i18n></p>
        </div>
      </div>
      <div class="column">
        <div class="rule">
          <p class="percent">{{ group.memberRemovalPercentage }}%</p>
          <circle-slider
            class="circle-slider"
            :value="group.memberRemovalPercentage"
            @input="value => update('memberRemovalPercentage', value)"
            :side="160"
            :circleWidth="1"
            :progressWidth="2"
            :knobRadius="5"
            :progressColor="removeColor"
            :knobColor="removeColor"
            circleColor="#69707a"
          >
          </circle-slider>
          <p class="title is-6"><i18n>Remove Member</i18n></p>
        </div>
      </div>
    </div>
    <transition name="open">
      <div class="message is-warning" v-if="!superMajority">
        <div class="message-body">
          <i18n>The percentage value you are choosing is most likely too low
          for a decision that can have a potentially significant impact
          on a person’s life. Please consider using a supermajority threshold.</i18n>
          <a href="https://groupincome.org/2016/09/deprecating-mays-theorem/#when-majority-rule-can-harm">
            <i18n>Read more on our blog about the dangers of majority rule.</i18n>
          </a>
        </div>
      </div>
    </transition>
  </div>
</template>
<style>
  .rule {
    position: relative;
    width: 160px;
    height: 140px;
    margin: 0 auto;
    padding-top: 40px;
  }
  .percent {
    font-size: 3rem;
    font-weight: bold;
    line-height: 3.4rem;
  }
  .circle-slider {
    position: absolute;
    top: 0;
    left: 0;
  }
  .message {
    margin-top: 2rem;
  }
  #rulesStep .message-body {
    border-color: #f68b39;
  }
  /* message transition */
  .open-enter-active, .open-leave-active {
    transition: max-height .3s ease;
    max-height: 150px;
    overflow: hidden;
  }
  .open-enter, .open-leave-to {
    max-height: 0;
  }
</style>
<script>
import { CircleSlider } from 'vue-circle-slider'

const SUPERMAJORITY = 67
const OKCOLOR = '#78c848'
const WARNCOLOR = '#f68b39'

export default {
  name: 'CreateGroupRules',
  props: {
    group: {type: Object},
    v: {type: Object}
  },
  components: {
    CircleSlider
  },
  methods: {
    update (prop, value) {
      this.v[prop].$touch()
      this.$emit('input', {
        data: {
          [prop]: value
        }
      })
    }
  },
  computed: {
    changeColor: function () {
      return this.group.changePercentage >= SUPERMAJORITY ? OKCOLOR : WARNCOLOR
    },
    approveColor: function () {
      return this.group.memberApprovalPercentage >= SUPERMAJORITY ? OKCOLOR : WARNCOLOR
    },
    removeColor: function () {
      return this.group.memberRemovalPercentage >= SUPERMAJORITY ? OKCOLOR : WARNCOLOR
    },
    superMajority: function () {
      return this.group.changePercentage >= SUPERMAJORITY &&
             this.group.memberApprovalPercentage >= SUPERMAJORITY &&
             this.group.memberRemovalPercentage >= SUPERMAJORITY
    }
  }
}
</script>

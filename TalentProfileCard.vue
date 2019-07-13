<template lang="pug">
  .tpc-container.tpci-animation-fade-in
  
    .tpc-modal-buttons
      i.tpc-action.material-icons( @click="closeModal" )
        | close
      i.tpc-action.material-icons 
        | open_in_new

    TalentProfileCardPersona( :talentData="talentData" )

    template( v-if="success" )

      .tpc-social-account-metrics( v-for="(insight, platform) in talentProfileCard" )
        TalentProfileCardInsightsRow( :value="insight" :platform="platform" :talentData="talentData")

    template( v-else-if="loading" )

      LoadingSmall

    template( v-else )
      Error


</template>



<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator"
import { Getter, Action, namespace as vuexModule } from 'vuex-class'

import api from '../../api/index'
import TalentProfileCardInsightsRow from "./TalentProfileCardInsightsRow.vue"
import TalentProfileCardPersona from "./TalentProfileCardPersona.vue"
import LoadingSmall from '../loading-small.vue'
import Error from '../error.vue'


const tpc = 'talentProfileCard'

@Component({
  components: {
    TalentProfileCardInsightsRow,
    TalentProfileCardPersona,
    LoadingSmall,
    Error
  }
})
export default class TalentProfileCard extends Vue {

  @Prop() talent
  @Prop() token
  @Prop() isBrandView

  public talentData = this.talent

  public talentProfileCard = null
  public loading = true
  public error  = false 
  public success = false


  created() {
    this.fetchTalentSummery()
  }

  fetchTalentSummery() {
    api.getWithoutToken('/talents/' + this.talent.id + '/insights/summary').then((response) => {
      this.talentProfileCard = response.data.data
      this.success = true
    })
    .catch(e => {
      if(e) {
        this.error = e
      }
      this.error = true
    })
    .finally(() => {
      this.loading = false
    })
  }

  closeModal() {
    this.$emit('closeTPcard')
  }

}
</script>


<style lang="sass">
$metric-value-color: #228FC5

.tpc-container
  width: 705px
  height: auto
  padding: 10px
  margin: auto
  border-radius: 5px
  border: 1px solid #ccc
  text-align: center
  background-color: #fff
  box-shadow: 3px 0px 22px -4px rgba(179,179,179,1)

.tpc-modal-buttons
  display: flex
  justify-content: space-between
  color: #C6CBD4

.tpc-action
  transition: all .2s  
  &:hover
    cursor: pointer
    color: #9FA4AE
    
.tpc-social-account-metrics
  display: flex
  flex-direction: row
  justify-content: space-around
  margin-top: 15px

</style>

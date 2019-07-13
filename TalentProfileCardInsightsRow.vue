<template lang="pug">
  .tpcir-contianer
    a.tpci-metric.tpci-audience-size-metric( :href="links" )
      img.tpci-platform-logo( :src=' "/static/images/" + platform + "-logo-colored.svg" ')

      .tpci-audience-size(v-if="audienceSize")
        | {{ audienceSize | formatNumber(1, false, true) }}
      .tpci-metric-fallback( v-else ) --
      
    .tpci-metric.tpci-iscore-metric
      .tpci-metric-title
        | ISCORE
      .tpci-score( v-if="qualityScore" )
        | {{ formatScore(qualityScore) || "--" }}
      .tpci-iscore-benchmark( v-if="qualityScore" )
        | {{ getScoreBenchmark(qualityScore) }}
      .tpci-metric-fallback( v-else ) --

    .tpci-metric.tpci-top-audience-metric
      .tpci-metric-title
        | TOP <br> AUDIENCE
      .tpci-top-audince-container( v-if="topAudience" )
        .tpci-top-audience-country-value.tpci-top-audience-value( v-if="topAudienceCountry" )
          | {{ capitalize(topAudienceCountry) }}
        .tpci-top-audience-gender-value.tpci-top-audience-value( v-if="topAudienceGender" )
          | {{ capitalize(topAudienceGender) }}, {{ topAudienceAge }}

      .tpci-metric-fallback( v-else ) --

    .tpci-metric.tpci-eng-rate-for-photo-metric
      .tpci-metric-title 
        | ENGAGEMENT <br> RATE PHOTO
      .tpci-score( v-if="engagementRatePhoto" )
        | {{ engagementRatePhoto| formatNumber(1, false, true) }}%
      .tpci-metric-fallback( v-else ) --


    .tpci-metric.tpci-eng-rate-for-video-metric
      .tpci-metric-title 
        | ENGAGEMENT <br> RATE VIDEO
      .tpci-score( v-if="engagementRateVideo" )
        | {{ engagementRateVideo | formatNumber(1, false, true) }}%
      .tpci-metric-fallback( v-else ) --


    .tpci-metric.tpci-viwership-rate-metric
      .tpci-metric-title
        | VIEWERSHIP <br> RATE
      .tpci-score( v-if="viewershipRate" )
        | {{ viewershipRate | formatNumber(1, false, true) }}%
      .tpci-metric-fallback( v-else ) --


</template>


<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator"
import { SocialAccounts } from '../../entities/Talent/SocialAccounts';

@Component({})
export default class TalentProfileCardInsightsRow extends Vue {

  @Prop() value
  @Prop() platform!: string
  @Prop() talentData!: any

  get audienceSize() {
    if (!this.value) return null
    return (this.value.audience_size)
  }

  get qualityScore() {
    if (!this.value) return null
    return (this.value.quality_score)
  }

  get topAudience() {
    if (!this.value) return null
    return (this.value.top_audience)
  }

  get topAudienceCountry() {
    if (!this.value) return null
    return (this.value.top_audience && this.value.top_audience.country)
  }

  get topAudienceGender() {
    if (!this.value) return null
    return (this.value.top_audience && this.value.top_audience.gender)
  }
  
   get topAudienceAge() {
    if (!this.value) return null    
    return (this.value.top_audience && this.value.top_audience.age)
  }

  get engagementRatePhoto() {
    if (!this.value) return null
    return (this.value.engagement_rates && this.value.engagement_rates.photo)
  }

  get engagementRateVideo() {
    if (!this.value) return null
    return (this.value.engagement_rates && this.value.engagement_rates.video)
  }

  get viewershipRate() {
    if (!this.value) return null
    return (this.value.viewership_rate)
  }

   get instgramUsername() {
    if (!this.talentData.social.accounts[this.platform]) return null 
    return this.talentData.social.accounts[this.platform].username
  }

  get facebookPlatformId() {
    if (!this.talentData.social.accounts[this.platform]) return null 
    return this.talentData.social.accounts[this.platform].platform_id
  }

  get youtubePlatformId() {
    if (!this.talentData.social.accounts[this.platform]) return null 
    return this.talentData.social.accounts[this.platform].platform_id
  }

  get links() {
   if (!this.platform) return null
   switch (this.platform) {

     case SocialAccounts.Platforms.INSTAGRAM: {
       return this.instgramUsername ? `//instagram.com/${this.instgramUsername}` : null
       break;
     }

    case SocialAccounts.Platforms.FACEBOOK: {
      return this.facebookPlatformId ? `//facebook.com/${this.facebookPlatformId}` : null 
      break;
    }

    case SocialAccounts.Platforms.YOUTUBE: {
      return this.youtubePlatformId ? `//youtube.com/channel/${this.youtubePlatformId}` : null
      break;
    }

   }
   
  }

}

</script>

<style lang="sass">
$metric-value-color: #228FC5

.tpcir-contianer
  display: flex
  flex-direction: row

.tpci-audience-size-metric
  width: 90px

.tpci-audience-size
  margin-top: 9px
  color: #7a7b7c
  font-size: 1.7rem

.tpci-instagram-logo
  width: 25px

.tpci-metric
  display: flex
  flex-direction: column
  justify-content: space-between
  width: 100px
  height: 81px
  padding: 13px 2px 10px 2px
  margin: 3px
  border: 1px solid #F6F7F9
  border-radius: 5px
  overflow: hidden

.tpci-metric-title
  color: #3C4B5A
  font-size: 10px
  line-height: 13px
  
.tpci-score
  margin-top: 5px
  color: $metric-value-color
  font-size: 16px
  font-weight: 500
  line-height: 20px
  overflow: hidden 

.tpci-top-audience-value
  margin-top: 10px
  font-size: 12px
  color: $metric-value-color
  line-height: 5px

.tpci-top-audience-value::after
  content: ","

.tpci-top-audience-value:last-child:after
  content: " "

.tpci-iscore-benchmark
  color: #1E1E32
  font-size: 9px

.tpci-youtube-row-width
  max-width: 458px

.tpci-animation-fade-in
  animation: fadeIn ease .5s

.tpci-metric-fallback
  color: #DBE0E2
  font-size: 20px
  font-weight: 400

@keyframes fadeIn
  0%
    opacity: 0

  100%
    opacity: 1
</style>



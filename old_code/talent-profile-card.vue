<template>
  <div class="profile-card-container" id="profile-card-container">
    <div class="flex-space-between height-17">
      <i @click="updateModalVisibilityStatus" class="material-icons cursor-pointer talent-icons talent-icon-close" id="close-icon">close</i>
      <router-link :to="{ name: 'Talent', params: {id: talent.id} }" target= '_blank' v-if="isBrandView">
        <i class="material-icons cursor-pointer talent-icons">open_in_new</i>
      </router-link>
    </div>
    <div class="talent-data-conatiner">
      <div class="flex-row">
        <div class="img-container">
          <img class="talent-profile-pic" id="profile-pic" :src="setTalentProfilePic(talent)" @error="handleBrokenTalentImage(talent.id)" />
        </div>
        <div class="talent-info-container">
          <div class="talent-name">{{talent.name}}</div>
          <div class="talent-nickname">{{talent.nickname}}</div>
          <div class="flex-column" v-if="talent.description">
            <div class="description-title">description</div>
            <div class="description-text">{{talent.description}}</div>
          </div>
        </div>
      </div>
      <div class="margin-top-20 margin-bottom-20" v-if="status.success">
        <template v-if="talentProfileCard">
          <!-- instagram -->
          <div v-if="talentProfileCard.instagram" class="flex-row platform-data">
            <div class="audience-size flex-space-between">
              <a :href="setPlatformLink(talent, 'instagram')" target="_blank" class="flex-column space-between height-54 cursor-pointer">
                <div><img src="/static/images/instagram-logo-colored.png" class="width-25 cursor-pointer"></div>
                <div class="width-100-percent line-height-16 color-black">{{talentProfileCard.instagram.audience_size ? formatDataValue(talentProfileCard.instagram.audience_size) : '--'}}</div>
              </a>
            </div>
            <div class="flex-space-between width-96 line-height-16">
              <div class="data-label">iSCORE</div>
              <div class="data-value margin-5"
                :class="{'line-height-2-5em': !talentProfileCard.instagram.quality_score}">
                  {{formatScore(talentProfileCard.instagram.quality_score) || '--' }}
              </div>
              <div class="benchmark">{{getScoreBenchmark(talentProfileCard.instagram.quality_score)}}</div>
            </div>
            <div class="flex-space-between width-135">
              <div class="data-label line-height-14 margin-bottom-2">TOP AUDIENCE</div>
              <div class="data-text" v-if="talentProfileCard.instagram.top_audience">
                <div>{{talentProfileCard.instagram.top_audience.country === null ? null : capitalizeFirstLetter(talentProfileCard.instagram.top_audience.country, true)}}</div>
                <div>{{talentProfileCard.instagram.top_audience.gender === null ? null : capitalizeFirstLetter(talentProfileCard.instagram.top_audience.gender, true)}}</div>
                <div>{{talentProfileCard.instagram.top_audience.age === null ? null : capitalizeFirstLetter(talentProfileCard.instagram.top_audience.age, false)}}</div>
              </div>
              <div v-else class="data-value line-height-12">
                {{'--'}}
              </div>
            </div>
            <div class="flex-space-between width-96">
              <div class="data-label line-height-12">ENGAGEMENT RATE PHOTO</div>
              <div class="data-value line-height-14">{{talentProfileCard.instagram.engagement_rates && talentProfileCard.instagram.engagement_rates.photo ? formatDataValue(talentProfileCard.instagram.engagement_rates.photo, true) : '--'}}</div>
            </div>
            <div class="flex-space-between width-96">
              <div class="data-label line-height-12">ENGAGEMENT RATE VIDEO</div>
              <div class="data-value line-height-14">{{talentProfileCard.instagram.engagement_rates && talentProfileCard.instagram.engagement_rates.video ? formatDataValue(talentProfileCard.instagram.engagement_rates.video, true) : '--'}}</div>
            </div>
            <div class="flex-space-between width-96">
              <div class="data-label line-height-12">VIEWERSHIP<br> RATE</div>
              <div class="data-value line-height-14">{{talentProfileCard.instagram.viewership_rate ? formatDataValue(talentProfileCard.instagram.viewership_rate, true) : '--'}}</div>
            </div>
          </div>
          <!-- end instagram -->

          <!-- facebook -->
          <div v-if="talentProfileCard.facebook" class="flex-row platform-data margin-top-20">
            <div class="audience-size flex-space-between">
              <a :href="setPlatformLink(talent, 'facebook')" target="_blank" class="flex-column space-between height-54 cursor-pointer">
                <div><img src="/static/images/facebook-logo-colored.svg" class="width-25 cursor-pointer"></div>
                <div class="width-100-percent line-height-16 color-black">{{talentProfileCard.facebook.audience_size ? formatDataValue(talentProfileCard.facebook.audience_size) : '--'}}</div>
              </a>
            </div>
            <div class="flex-space-between width-96 line-height-16">
              <div class="data-label">iSCORE</div>
                <div class="data-value margin-5"
                :class="{'line-height-2-5em': !talentProfileCard.facebook.quality_score}">
                  {{formatScore(talentProfileCard.facebook.quality_score) || '--' }}
              </div>
                <div class="benchmark">{{getScoreBenchmark(talentProfileCard.facebook.quality_score)}}</div>
            </div>
            <div class="flex-space-between width-135">
              <div class="data-label line-height-14 margin-bottom-2">TOP AUDIENCE</div>
              <div class="data-text" v-if="talentProfileCard.facebook.top_audience">
                <!-- <div>{{capitalize(talentProfileCard.facebook.top_audience.country)}},</div> -->
                <div>{{talentProfileCard.facebook.top_audience.gender === null ? null : capitalizeFirstLetter(talentProfileCard.facebook.top_audience.gender,true)}}</div>
                <div>{{talentProfileCard.facebook.top_audience.aeg === null ? null : capitalizeFirstLetter(talentProfileCard.facebook.top_audience.age, false)}}</div>
              </div>
              <div v-else class="data-value line-height-12">
                {{'--'}}
              </div>
            </div>
            <div class="flex-space-between width-96">
              <div class="data-label line-height-12">ENGAGEMENT RATE PHOTO</div>
              <div class="data-value line-height-14">{{talentProfileCard.facebook.engagement_rates && talentProfileCard.facebook.engagement_rates.photo ? formatDataValue(talentProfileCard.facebook.engagement_rates.photo, true) : '--'}}</div>
            </div>
            <div class="flex-space-between width-96">
              <div class="data-label line-height-12">ENGAGEMENT RATE VIDEO</div>
              <div class="data-value line-height-14">{{talentProfileCard.facebook.engagement_rates && talentProfileCard.facebook.engagement_rates.video ? formatDataValue(talentProfileCard.facebook.engagement_rates.video, true) : '--'}}</div>
            </div>
            <div class="flex-space-between width-96">
              <div class="data-label line-height-12">VIEWERSHIP<br> RATE</div>
              <div class="data-value line-height-14">{{talentProfileCard.facebook.viewership_rate ? formatDataValue(talentProfileCard.facebook.viewership_rate, true) : '--'}}</div>
            </div>
          </div>
          <!-- end facebook -->

          <!-- youtube -->
          <div v-if="talentProfileCard.youtube" class="flex-row platform-data margin-top-20">
            <div class="audience-size flex-space-between">
              <a :href="setPlatformLink(talent, 'youtube')" target="_blank" class="flex-column space-between height-54 cursor-pointer">
                <div><img src="/static/images/youtube-logo-colored.svg" class="width-34 cursor-pointer"></div>
                <div class="width-100-percent line-height-16 color-black">{{talentProfileCard.youtube.audience_size ? formatDataValue(talentProfileCard.youtube.audience_size) : '--'}}</div>
              </a>
            </div>
            <div class="flex-space-between width-96 line-height-16">
              <div class="data-label">iSCORE</div>
              <div class="data-value margin-5"
                :class="{'line-height-2-5em': !talentProfileCard.youtube.quality_score}">
                  {{formatScore(talentProfileCard.youtube.quality_score) || '--' }}
              </div>
              <div class="benchmark">{{getScoreBenchmark(talentProfileCard.youtube.quality_score)}}</div>
            </div>
            <div class="flex-space-between width-135">
              <div class="data-label line-height-14 margin-bottom-2">TOP AUDIENCE</div>
              <div class="data-text" v-if="talentProfileCard.youtube.top_audience">
                <div>{{talentProfileCard.top_audience.country == null ? null : capitalizeFirstLetter(talentProfileCard.youtube.top_audience.country, true)}}</div>
                <div>{{talentProfileCard.top_audience.country == null ? null : capitalizeFirstLetter(talentProfileCard.youtube.top_audience.gender, true)}}</div>
                <div>{{talentProfileCard.top_audience.country == null ? null : capitalizeFirstLetter(talentProfileCard.youtube.top_audience.age, false)}}</div>
              </div>
              <div v-else class="data-value line-height-12">
                {{'--'}}
              </div>
            </div>
            <div class="flex-space-between width-96">
              <div class="data-label line-height-12">ENGAGEMENT RATE</div>
              <div class="data-value line-height-14">{{talentProfileCard.youtube.engagement_rates ? formatDataValue(talentProfileCard.youtube.engagement_rates.video, true) : '--'}}</div>
            </div>
          </div>
          <!-- end youtube -->
        </template>
        <template v-else>
          <div class="flex-center-center desc-text">No data available at the moment, please try again later.</div>
        </template>
      </div>
      <div v-else-if="status.error">
        <error></error>
      </div>
      <div v-else>
        <loading-small></loading-small>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
import LoadingSmall from '@/components/loading-small'
import Error from '@/components/error'

export default {
  name: 'talent-profile-card',
  data () {
    return {
      notAvailable: 'N/A'
    }
  },
  props: {
    talent: {
      type: Object,
      required: true
    },
    token: {
      type: String
    },
    triggererId: {
      type: String,
      required: true
    },
    /*
    this property is responsible for updating the visibility of the modal
     */
    modalVisibilityStatusFunction: null,
    /*
    this is used to determine if the full profile button should be visible or not
    and if the default api or guests api should be called
     */
    isBrandView: {
      type: Boolean,
      default: true
    }
  },
  components: {
    LoadingSmall,
    Error
  },
  created () {
    if (this.isBrandView) {
      this.fetchTalentProfileCard({talentId: this.talent.id})
    } else {
      this.fetchGuestsTalentProfileCard({talentId: this.talent.id, token: this.token})
    }
  },
  mounted () {
    this.addListeners()
  },
  beforeDestroy() {

  },
  methods: {
    ...mapActions({
      fetchTalentProfileCard: 'talentProfileCard/fetchTalentProfileCard',
      fetchGuestsTalentProfileCard: 'talentProfileCard/fetchGuestsTalentProfileCard'
    }),

    handleBrokenTalentImage (id) {
      let talentImage = document.getElementById("profile-pic")
      talentImage.src = '/static/images/talent-photo-placeholder.svg'
    },

    //  capitalize the first letter of each top audience metric
    capitalizeFirstLetter (string, addComa) {
      if (string && addComa) {
        return string.charAt(0).toUpperCase() + string.slice(1) + ','
      } else if (string && !addComa) {
        return string.charAt(0).toUpperCase() + string.slice(1)
      }
      return null
    },

    addListeners() {
      document.addEventListener('keydown', this.onEscape)
      document.addEventListener('click', this.onClickOutside)
    },
    removeListeners() {
      document.removeEventListener('keydown', this.onEscape)
      document.removeEventListener('click', this.onClickOutside)
    },
    onEscape(evt) {
      if (evt.keyCode !== 27) return
      this.removeListeners()
      this.updateModalVisibilityStatus()
    },
    onClickOutside(evt) {
      const flyoutElement = document.getElementById('profile-card-container')
      var targetElement = evt.target; // clicked element
      do {
          if (targetElement === flyoutElement) {
            // This is a click inside. Do nothing, just return.
            return
          }
          targetElement = targetElement.parentNode
      } while (targetElement)
      if (evt.target.id !== this.triggererId) {
        this.removeListeners()
        this.updateModalVisibilityStatus()
      }
    },
    formatDataValue (value, isPercentage = false) {
      let returnValue = this.notAvailable
      if (value) {
        if (value !== 0) {
          if (isPercentage) {
            returnValue = this.formatNumber(value, 1, false, true) + '%'
          } else {
            returnValue = this.formatNumber(value, 1)
          }
        } else {
          returnValue = '--'
        }
      }
      return returnValue
    },
    updateModalVisibilityStatus () {
      this.modalVisibilityStatusFunction(false)
    },
    setPlatformLink (talent, platform) {
      let platformLink = ''
      if (talent.social.accounts[platform]) {
        if (talent && platform === 'instagram') {
          platformLink = 'http://www.' + platform + '.com/' + talent.social.accounts[platform].username
        } else if (talent && platform === 'facebook') {
          platformLink = 'http://www.' + platform + '.com/' + talent.social.accounts[platform].platform_id
        } else if (talent && platform === 'youtube') {
          platformLink = 'http://www.' + platform + '.com/channel/' + talent.social.accounts[platform].platform_id
        }
      }
      return platformLink
    }
  },
  computed: {
    ...mapGetters({
      talentProfileCard: 'talentProfileCard/talentProfileCard',
      status: 'talentProfileCard/status'
    })
  }
}
</script>

<style scoped>
.profile-card-container {
  font-family: Roboto;
  margin: 0px auto;
  height: auto;
  width: 705px;
  border: 1px solid #F6F7F9;
  border-radius: 5px;
  background-color: #FFFFFF;
  padding: 11px 11px 5px 11px;
  cursor: initial;
}
.talent-data-conatiner {
  padding:0 15px;
}
.talent-icons {
  color: #C6CBD4;
}
.talent-icon-close {
  font-size: 30px;
  line-height: 22px;
}
.talent-profile-pic {
  box-sizing: border-box;
  width: 121px;
  border-radius: 50%;
}
.img-container {
  width: 121px;
  margin-right: 20px;
}
.talent-info-container {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  flex-direction: column;
  width: 100%;
}
.talent-name {
  color: #212121;
  font-size: 24px;
  font-weight: 900;
  line-height: 32px;
  white-space: nowrap;
  text-align: left;
  width: 80%;
  overflow: hidden;
  text-overflow: ellipsis;
}
.talent-nickname {
  color: #C0C0C0;
  font-size: 14px;
  line-height: 19px;
  width: 80%;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.description-title {
  height: 19px;
  width: 126px;
  color: #4A4A4A;
  font-size: 11px;
  font-weight: 900;
  line-height: 17px;
  text-transform: uppercase;
  margin-top: 13px;
}
.description-text {
  width: 100%;
  color: #4A4A4A;
  font-size: 11px;
  font-weight: 300;
  line-height: 15px;
  width: 460px;
}
.talent-platform-data {
  margin-top: 24px;
}
.platform-data > div {
  display: flex;
  flex-direction: column;
  height: 80px;
  border: 1px solid #F6F7F9;
  border-radius: 5px;
  margin-right: 4px;
  padding: 12px 0;
  align-items: center;
  text-align: center;
}
.audience-size {
  width: 114px;
  color: #000000;
  font-size: 25px;
}
.audience-size > div {
  font-weight: 300;
}
.width-25 {
  width: 25px;
}
.width-34 {
  width: 34px;
}
.width-96 {
  width: 96px;
}
.width-135 {
  width: 135px;
}
.data-label {
  color: #3C4B5A;
  font-size: 10px;
}
.data-value {
  color: #228FC5;
  font-size: 20px;
  font-weight: 500;
}
.data-text {
  color: #1381C9;
  font-size: 12px;
  line-height: 14px;
}
.benchmark {
  color: #1E1E32;
  font-size: 9px;
}
.line-height-2-5em {
  line-height: 2.5em;
}

.line-height-16 {
  line-height: 16px;
}
.line-height-30 {
  line-height: 30px !important;
}
.min-height-120 {
  min-height: 120px;
}
.height-54 {
  height: 54px;
}
.space-between {
  justify-content: space-between;
}
.margin-5 {
  margin: 5px 0;
}
.margin-bottom-2 {
  margin-bottom: 2px;
}
.width-100-percent {
  width: 100% !important;
}
.color-black {
  color: #000000 !important;
}
</style>

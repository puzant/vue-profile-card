<template lang="pug">
.talent-profile-card-persona

  .tpcp-inner-container
    .tpcp.tpcp-image-container
      img( class="tpcp-image" :src = "getProfilePicture()" )
    .tpcp.tpcp-name-nickname-contianer
      .tpcp-name
        | {{ talentName }}
      .tpcp-nickname
        | {{ talentNickName }}
      .tpcp-gender( v-if="talentGender" )
        | {{ talentGender }}

  .tpcp-social-info-container
    .tpcp-contact-info-container( v-for="(contact, contactType) in talentContacts" )
      img.tpcp-contact-logo( v-if="contact" :src=' "/static/images/" + contactType + ".svg" ' )
      a.tpcp-contact-info( :href="setContactLink(contactType)" )
        | {{ contact }}

  .tpcp-talent-intrests
    .tpcp-intrest( v-for="intrest in talentIntrests" )
      | {{ intrest.title }}
  
</template>


<script lang="ts">

import { Component, Prop, Vue } from "vue-property-decorator";

enum ContactType {
  EMAIL = 'email',
  PHONE = 'phone',
  SNAPCHAT_USERNAME = 'snapchat_username',
  TWITTER_HANDLE = 'twitter_handle'
}

@Component ({})
export default class TalentPersonna extends Vue {
  @Prop() talentData!: any

  public socialData = this.talentData.external_sources.social_data
  /**
   * talentContacts: Object => the talent contact object
   * contactKey: String => check if this key exsits
   */
  public checkForContactInfo(talentContacts, contactKey): boolean {
    if (talentContacts[contactKey]) {
      return true 
    }
    return false
  }

  public getProfilePicture(): string {
    if (this.talentData.social.accounts.instagram) {
      return this.talentData.social.accounts.instagram.profile_picture
    }
    return "/static/images/talent-photo-placeholder.svg"
  }

  get talentName() {
    if (!this.talentData) return null
    return this.talentData.name
  }

  get talentNickName() {
    if (!this.talentData) return null
    return this.talentData.nickname
  }

  get talentGender() {
    return (this.talentData && this.talentData.gender) || (this.socialData && this.socialData.gender) || null
  }

  get talentContacts() {
    if (!this.socialData|| !this.socialData.contact) return null
    return this.talentData.external_sources.social_data.contact
  }

  get talentIntrests() {
    if (!this.socialData || !this.socialData.interests) return null
    return this.socialData.interests
  }

  talentContactInfo(contactType) {
    return this.talentData.external_sources.social_data.contact[contactType]
  }

  setContactLink(contactType) {

    switch (contactType) {

      case ContactType.EMAIL: 
        return "mailto:" + this.talentContactInfo(contactType)

      case ContactType.PHONE:
        return "tel:" + this.talentContactInfo(contactType)

      case ContactType.SNAPCHAT_USERNAME:
        return '//snapchat.com/add/' + this.talentContactInfo(contactType)
      
      case ContactType.TWITTER_HANDLE:
        return '//twitter.com/' + this.talentContactInfo(contactType)

    }

  }

}

</script>

<style lang="sass">
$tpcp-title-font-weight: 900
$tpcp-description-title-font-size: 11px

.talent-profile-card-persona
  display: flex
  flex-direction: column
  justify-content: flex-start

.tpcp-inner-container
  display: flex
  flex-direction: row

.tpcp
  margin: 12px

.tpcp-image-container
  border-radius: 50%

.tpcp-image 
  width: 62px
  border: 1px solid #ccc
  border-radius: 50%

.tpcp-name-nickname-contianer
  display: flex
  flex-direction: column
  justify-content: center
  width: 100%
  margin: 0 0 0 auto
  text-align: left

.tpcp-name
  color: #3C4B5A
  font-size: 24px
  font-weight: $tpcp-title-font-weight
  text-overflow: elipses

.tpcp-description-title
  color: #3C4B5A
  font-size: $tpcp-description-title-font-size
  font-weight: $tpcp-title-font-weight
  text-transform: uppercase

.tpcp-nickname
  margin: 1px
  width: 460px
  color: #C0C0C0
  font-size: 14px
  font-weight: 300
  line-height: 19px

.tpcp-gender
  color: #3C4B5A
  font-size: 10px
  line-height: 15px

.tpcp-social-info-container
  display: flex
  flex-direction: row
  justify-content: flex-start

.tpcp-contact-info-container
  display: flex
  flex-direction: row
  justify-content: flex-start
  margin: 5px

.tpcp-contact-info
  color: #3C4B5A
  font-size: 12px
  font-wieght: 300
  line-height: 18px

.tpcp-contact-logo
  margin: 3px

.tpcp-talent-intrests
  display: flex
  flex-direction: row
  flex-wrap: wrap
  margin: 0 0 0 5px
  
.tpcp-intrest
  width: auto
  padding: 0 6px
  margin: 2px
  color: #3C4B5A
  background-color: #E9EAED
  font-size: 12px
  font-weight: 300
  border-radius: 2px
</style>

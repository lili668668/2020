<!--
  Copyright (c) 2020 DevilTea

  This software is released under the MIT License.
  https://opensource.org/licenses/MIT
-->

<template>
  <section id="sponsor-footer">
    <div class="outer-container">
      <div
        v-for="sponsor in sponsors"
        :key="sponsor.id"
        class="sponsor"
        target="_blank"
        rel="noopener"
      >
        <a :href="`${sponsor.link}`" target="_blank" rel="noopener">
          <img
            :alt="sponsor.name"
            :src="`/2020/images/sponsors/${sponsor.image}`"
          />
        </a>
      </div>
    </div>
  </section>
</template>

<script lang="ts">
import Vue from 'vue'
import { groupBy } from 'lodash-es'
import sponsorDatas from '@/../public/json/sponsor.json'

export default Vue.extend({
  name: 'SponsorFooter',
  data () {
    return {
      sponsors: [] as unknown[]
    }
  },
  methods: {
    async initSponsors () {
      this.sponsors = Object.entries(groupBy(sponsorDatas, 'level'))
        .sort((entryA, entryB) => {
          const sponsorSequence = ['titanium', 'diamond', 'gold', 'silver', 'bronze', 'co-organizer', 'special-thanks']
          return sponsorSequence.indexOf(entryA[0]) - sponsorSequence.indexOf(entryB[0])
        })
        .flatMap((entry) => {
          return entry[1]
        })
        .map((sponsor) => ({
          id: sponsor.id,
          name: sponsor.name,
          link: sponsor.link,
          image: sponsor.image
        }))
    }
  },
  async mounted () {
    await this.initSponsors()
    this.$dispatchRenderedEvent()
  }
})
</script>

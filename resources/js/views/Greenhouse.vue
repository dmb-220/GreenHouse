<template>
  <div>
    <hero-bar>
      GreenHouse
    </hero-bar>
    <section class="section is-main-section">
      <card-component title="VALDYMAS" icon="finance">
	  INFO
      </card-component>
    </section>
  </div>

</template>

<script>

import CardComponent from '@/components/CardComponent'
import HeroBar from '@/components/HeroBar'
export default {
  name: 'GreenHouse',
  components: { HeroBar, CardComponent },
  computed: {
  }
}
</script>

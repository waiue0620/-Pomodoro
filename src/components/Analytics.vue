<template>
  <div class="pb-5">
    <section class="mb-5">
      <div class="d-flex h4 px-3 py-2 whiteOpacity20">focus time</div>
      <div class="row mt-3">
        <div class="col-6">
          <p class="h6 m-0">today</p>
          <p class="text-primary m-0" style="font-size:64px">20<span class="text-white20 ml-1" style="font-size:16px">/tomato</span></p>
        </div>
        <div class="col-6">
          <p class="h6 m-0">week</p>
          <p class="text-primary m-0" style="font-size:64px">108<span class="text-white20 ml-1" style="font-size:16px">/tomato</span></p>
        </div>
      </div>
    </section>
    <section>
      <div class="d-flex h4 px-3 py-2 whiteOpacity20 mb-3">chart</div>
        <chart class="wrapper"></chart>
    </section>
  </div>
</template>

<script>
import chart from '@/components/Chart'

export default {
  components: {
    chart
  }
}
</script>

<style>
.wrapper{
  height: 300px;
}

</style>

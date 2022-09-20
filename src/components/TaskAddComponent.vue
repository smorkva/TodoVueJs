<template>
  <div class="row">
    <div class="col-7">
      <input type="text" v-model="title" class="form-control"/>
    </div>
    <div class="col-5 text-left">
      <button
        type="button"
        class="btn btn-secondary"
        @click="$emit('save')"
      >Submit</button>
    </div>
  </div>
</template>

<script lang="ts">
  import { Component, VModel, Vue } from 'vue-property-decorator';

  @Component
  export default class TaskAdd extends Vue {
    @VModel({ type: String }) title!: string;
  }
</script>

<template>
	<div class="row">
		<div class="col-7">
			<input type="text" class="form-control" v-model="title"  />
		</div>
		<div class="col-5">
			<button type="button" class="btn btn-secondary" @click="$emit('save')">EDIT</button>
			<button type="button" class="btn btn-info" @click="$emit('cancel')">Cancel</button>
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

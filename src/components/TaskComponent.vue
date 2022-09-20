<template>
  <li class="task list-group-item d-flex justify-content-between align-items-center">
		<span v-text="task.title"></span>
		<button
			type="button"
			class="btn btn-success"
			@click="$emit('edit')"
		>Edit</button>
		<button
			type="button"
			class="btn btn-danger"
			@click="$emit('delete')"
		>Delete</button>
	</li>
</template>

<script lang="ts">
	import { Component, Prop, Vue } from 'vue-property-decorator';
	import { task as taskModel } from '../models/task';
	
	@Component
	export default class Task extends Vue {
		@Prop() public task!: taskModel;
	}
	</script>

<style scoped>
	.task span {
		text-align: left;
		flex-grow: 2;
	}
</style>
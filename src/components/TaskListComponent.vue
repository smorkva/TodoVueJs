<template>
	<div>
		<h3 class="text-left">Tasks</h3>
		<ul class="list-group">
			<task-component
				v-for="task in tasks"
				:key="task.id"
				:task="task"
				@edit="$emit('edit', task)"
				@delete="$emit('delete', task)"
			/>
		</ul>
	</div>
</template>

<script lang="ts">
	import { Component, Prop, Vue } from 'vue-property-decorator';
	import { task as taskModel } from '../models/task';
	import TaskComponent from './TaskComponent.vue';
	
	@Component({
		components: {
			TaskComponent,
		},
	})

	export default class TaskList extends Vue {
		@Prop() public tasks!: taskModel[];
	}
</script>

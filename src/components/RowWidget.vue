<template>
	<div class="row">
		<component class="result__component__row" :data="com.data" :is="com.is" v-for="com in data.children" :key="com.name"></component>
	</div>
</template>
<script lang="ts">
import Vue from "vue";
import TitleWidget from "@/components/TitleWidget.vue";
import EditWidget from "@/components/EditWidget.vue";

export default Vue.extend({
	name: "RowWidget",
	components: {
		TitleWidget,
		EditWidget
	},
	props: {
		data: Object
	}
});
</script>
<style scoped>
.row {
	display: flex;
	flex-direction: row;
    padding: 10px;
}
</style>
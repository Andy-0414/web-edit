<template>
	<div class="editor">
		<div class="panel">
			<div class="panel__setting">
				Width :
				<input type="number" v-model="grid" />
			</div>
			<div class="widgetList">
				<div
					class="widgetList__item"
					v-for="item in list"
					:key="item"
					@mousedown="select($event,item)"
				>{{item}}</div>
			</div>
		</div>

		<div class="result" ref="body">
			<component
				class="result__component"
				:data="com.data"
				:is="com.is"
				v-for="com in result"
				:key="com.name"
			></component>
		</div>
		<div ref="example" class="example">
			<component v-if="currentSelect" :is="currentSelect.is" :data="currentSelect.data"></component>
		</div>
	</div>
</template>
<script lang="ts">
import Vue from "vue";
import TitleWidget from "@/components/TitleWidget.vue";
import EditWidget from "@/components/EditWidget.vue";
import RowWidget from "@/components/RowWidget.vue";

interface WebForm {
	name: string;
	is: string;
	data: {
		children?: WebForm[];
		text?: string;
	};
}

export default Vue.extend({
	name: "Home",
	data() {
		return {
			example: document.createElement("div") as HTMLDivElement,
			list: ["TitleWidget", "EditWidget", "RowWidget"],
			result: [
				{
					name: "test1",
					is: "RowWidget",
					data: {
						children: [
							{
								name: "test2",
								is: "TitleWidget",
								data: {
									text: "TEST2"
								}
							},
							{
								name: "test3",
								is: "EditWidget",
								data: {
									text: "TEST3"
								}
							}
						]
					}
				}
			] as WebForm[],
			grid: 1280,
			currentSelect: {} as WebForm
		};
	},
	components: {
		TitleWidget,
		EditWidget,
		RowWidget
	},
	mounted() {
		this.example = this.$refs.example as HTMLDivElement;
		addEventListener("mousemove", e => {
			if (this.currentSelect.name) {
				let find: any = document.elementsFromPoint(
					e.clientX,
					e.clientY
				);
				let findX =
					find.filter((x: Element) => {
						return x.classList.contains("result__component__row");
					})[0] || null;
				let findY =
					find.filter((x: Element) => {
						return x.classList.contains("result__component");
					})[0] || null;
				if (findX) {
					if (e.clientX - findX.offsetLeft > findX.offsetWidth / 2) {
						findX.style.marginRight =
							this.example.offsetWidth + "px";
						this.moveExample(
							findX.offsetLeft + findX.offsetWidth,
							findX.offsetTop
						);
					} else {
						findX.style.marginLeft =
							this.example.offsetWidth + "px";
						this.moveExample(
							findX.offsetLeft - this.example.offsetWidth,
							findX.offsetTop
						);
					}
				} else if (findY) {
				} else {
					this.moveExample(
						e.clientX - this.example.offsetWidth / 2,
						e.clientY - this.example.offsetHeight / 2
					);
				}
			}
		});
		addEventListener("mouseup", e => {
			console.log();
			this.currentSelect = {} as WebForm;
		});
	},
	watch: {
		grid(value) {
			(this.$refs.body as HTMLDivElement).style.maxWidth = value + "px";
		}
	},
	methods: {
		select(e: MouseEvent, item: string) {
			this.moveExample(e.clientX, e.clientY);
			this.currentSelect = {
				name: item,
				is: item,
				data: {
					text: "Hello"
				}
			};
		},
		moveExample(x: number, y: number) {
			this.example.style.left = x + "px";
			this.example.style.top = y + "px";
		},
		put() {
			console.log("PUT");
		}
	}
});
</script>
<style>
.editor {
	width: 100vw;
	height: 100vh;
	display: flex;
	justify-content: center;
}
.panel {
	position: fixed;
	right: 50px;
	top: 50px;
	width: 400px;
	height: 600px;

	box-shadow: 0px 0px 10px #33333333;

	display: flex;
	flex-direction: column;

	padding: 10px;
	z-index: 100;
	background-color: white;
}
.widgetList {
	flex: 1;
	user-select: none;
}
.widgetList__item {
	width: 100%;
	padding: 20px;
	text-align: center;
}
.widgetList__item:hover {
	background-color: #eeeeee;
}

.result {
	width: 100%;
	max-width: 1280px;
	border: 1px solid #eeeeeeee;
}
.example {
	cursor: grab;
	position: absolute;
	top: 0;
	left: 0;
	z-index: 101;
	opacity: 0.5;
    transition: 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    background-color: #eeeeee;
    border-radius: 10px;
}
.result__component,.result__component__row{
    transition: 0.5s cubic-bezier(0.175, 0.885, 0.32, 1.275);
    box-sizing: border-box !important;
}
.result__component:hover,.result__component__row:hover{
    background-color: #36AFFF33
}
</style>
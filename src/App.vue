<template>
	<div class="app" :class="{ 'app--overlay': isPopupActive }">
		<button
			v-if="!isPopupActive"
			class="app__btn-activator"
			@click="togglePopup"
		>
			Налоговый вычет
		</button>
		<PopupCalc v-else @close="togglePopup" />
	</div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator"
import PopupCalc from "./components/PopupCalc/PopupCalc.vue"

@Component({
	components: {
		PopupCalc,
	},
})
export default class App extends Vue {
	isPopupActive: boolean = false

	togglePopup() {
		document.body.style.overflowY = !this.isPopupActive ? "hidden" : "auto"
		this.isPopupActive = !this.isPopupActive
	}
}
</script>

<style lang="scss">
@import "~normalize.css/normalize.css";
@import "assets/scss/fonts";
@import "assets/scss/style";

.app {
	display: flex;
	justify-content: center;
	align-items: center;
	width: 100vw;
	height: 100vh;
	background: linear-gradient(
			255.35deg,
			#dc3131 0.83%,
			rgba(255, 79, 79, 0) 108.93%
		),
		#ff5e56;
	box-shadow: 0px -0.11px 16.9495px rgba(183, 187, 225, 0.33);

	&--overlay {
		background: #fff;
	}
}
.app__btn-activator {
	padding: 12px 24px;
	font-weight: 500;
	font-size: 16px;
	line-height: 24px;
	color: #fff;
	border: 1px solid #ffffff;
	border-radius: 6px;
	box-sizing: border-box;
	filter: drop-shadow(0px 0px 44px #cfdae7);
	transition: all 0.3s;

	&:active,
	&:hover {
		background: #fff;
		filter: none;
		color: #000;
	}
}
</style>

<template>
	<div class="popup-calc__total">
		<p class="popup-calc__total-text">
			Итого можете внести в качестве досрочных:
		</p>
		<ul class="popup-calc__list">
			<li v-for="(deduction, index) in deductions" :key="deduction / index">
				<label class="popup-calc__checkbox">
					<input type="checkbox" />
					<div class="popup-calc__checkbox-body"></div>
					<div class="popup-calc__checkbox-text">
						{{ deduction.toLocaleString(index + 1) }} рублей
						{{ index + 1 === 2 ? "во" : "в" }}
						{{ index + 1 + "-" + createEndOfWord(index + 1) }} год
					</div>
				</label>
			</li>
		</ul>
	</div>
</template>
<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator"

@Component
export default class PopupCalc extends Vue {
	@Prop() deductions!: number[]

	createEndOfWord(num: number) {
		if (
			(num > 5 && num < 9) ||
			(num > 20 && num % 10 > 5 && num % 10 < 9) ||
			num === 2
		)
			return "ой"
		if (+num.toString().slice(-2) != 13 && num % 10 === 3) return "ий"
		return "ый"
	}
}
</script>

<style lang="scss">
.popup-calc__total {
	padding-bottom: 20px;
	margin-top: -8px;
}
.popup-calc__total-text {
	font-weight: 500;
	margin-top: 0;
	margin-bottom: 16px;
}
.popup-calc__list {
	li {
		padding: 16px 0;
		border-bottom: 1px solid #dfe3e6;

		&:first-child {
			padding-top: 0;
		}
	}
}
.popup-calc__checkbox {
	display: flex;
	align-items: center;
	width: max-content;
	cursor: pointer;

	&:hover .popup-calc__checkbox-body {
		border-color: #000;
	}
	&:disabled {
		background-color: #bec5cc;
		border-color: #bec5cc;
	}

	.popup-calc__checkbox-body {
		width: 20px;
		height: 20px;
		margin-right: 11px;
		background-color: #fff;
		border-radius: 6px;
		border: 1px solid #dfe3e6;
	}
	input {
		display: none;
		&:checked ~ .popup-calc__checkbox-body {
			background: linear-gradient(
					255.35deg,
					#dc3131 0.83%,
					rgba(255, 79, 79, 0) 108.93%
				),
				#ff5e56;
			background-image: url("../../assets/img/check.png");
			background-repeat: no-repeat;
			background-position: center center;
			border-color: transparent;
		}
	}
}
</style>

<template>
	<div class="popup-calc">
		<div class="popup-calc__wr">
			<div class="popup__close" @click="$emit('close')">
				<img src="@/assets/img/close.png" alt="close" />
			</div>
			<div class="popup-calc__content">
				<div>
					<h2 class="popup-calc__title">Налоговый вычет</h2>
					<p class="popup-calc__description">
						Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер
						налогового вычета составляет не более 13% от своего официального
						годового дохода.
					</p>
					<div class="popup-calc__input-field" :class="{ error: isEmpty }">
						<label>
							<p>Ваша зарплата в месяц</p>
							<input
								type="text"
								placeholder="Введите данные"
								@input="replaceSalaryValue"
								@change="replaceSalaryValue"
								@keydown="pressEnterHandler"
								v-model="formatedSalary"
							/>
						</label>
						<p v-if="isEmpty" class="popup__error">
							Поле обязательно для заполнения
						</p>
					</div>
					<button class="popup-calc__start" @click="calc">Рассчитать</button>
					<PopupCalcTotal v-if="deductions.length" :deductions="deductions" />
					<div class="popup-calc__type">
						<span class="popup-calc__text">Что уменьшаем?</span>
						<div class="popup-calc__radio-groop">
							<label class="popup-calc__radio">
								<input type="radio" name="type" checked />
								<div class="popup-calc__radio-body">Платёж</div>
							</label>
							<label class="popup-calc__radio">
								<input type="radio" name="type" />
								<div class="popup-calc__radio-body">Срок</div>
							</label>
						</div>
					</div>
				</div>
				<button class="popup-calc__add">Добавить</button>
			</div>
		</div>
	</div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator"
import PopupCalcTotal from "./PopupCalcTotal.vue"

@Component({
	components: {
		PopupCalcTotal,
	},
})
export default class PopupCalc extends Vue {
	isEmpty: boolean = false
	salary: number = 0
	formatedSalary: string = ""
	isChanged: boolean = false
	deductions: number[] = []

	replaceSalaryValue(e: any) {
		this.isEmpty = false
		this.isChanged = true

		if (e.data || e.type === "change") {
			const salary: string = this.formatedSalary.replace(/\D|₽/gi, "")

			const formatter: any = new Intl.NumberFormat("ru", {
				style: "currency",
				currency: "RUB",
				maximumFractionDigits: 0,
				minimumFractionDigits: 0,
			})
			this.salary = +salary
			this.formatedSalary = formatter.format(salary)
		}
	}

	calc() {
		if (!this.salary) return (this.isEmpty = true)

		if (this.isChanged) {
			const deductionsPerMonth = []
			const deduction = +this.salary * 12 * 0.13
			let max = 260000

			if (deduction < max) {
				while (max > 0) {
					if (max > deduction) {
						max -= deduction
						deductionsPerMonth.push(deduction)
					} else {
						deductionsPerMonth.push(max)
						max = 0
					}
				}
			} else deductionsPerMonth.push(260000)

			this.deductions = deductionsPerMonth
			this.isChanged = false
		}
	}

	pressEnterHandler(e: any) {
		if (e.keyCode === 13) {
			this.calc()
		}
	}
}
</script>

<style lang="scss" src="./PopupCalc.scss"></style>

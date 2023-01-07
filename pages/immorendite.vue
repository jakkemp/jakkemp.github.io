<script setup>
	const isAdvanced = ref(false)
	const colorTheme = ref('pastel')
	const colorStyle = computed(() => ({
		'bg-primary': !isAdvanced.value,
		'bg-secondary': isAdvanced.value
	}))

	const buyingPrice = ref(0)
	const isRentAnnualized = ref(false)
	const netRent = ref(0)
	const annualizedNetRent = computed(() => {
  		return isRentAnnualized.value == true ? netRent.value : netRent.value*12
	})
	const grossRentalYield = computed(() => {
		return annualizedNetRent.value/buyingPrice.value
	})
</script>

<template>
    <div :data-theme="colorTheme" class="min-h-screen bg-base-200 p-6">
        <div class="flex justify-center my-6">
            <div class="prose text-center">
				<h1>Immobilienrendite</h1>
			</div>
		</div>
		<div class="form-control w-64 ml-6 -mb-2">
			<label class="cursor-pointer label">
				<span class="label-text">Einfach</span> 
				<input type="checkbox" class="toggle toggle-accent" v-model="isAdvanced" />
				<span class="label-text">Fortgeschritten</span> 
			</label>
		</div>
		<div class="flex justify-between flex-wrap">
			<div class="grow w-5/12 min-w-min prose rounded-2xl p-6 m-6" :class="colorStyle">
				<h2 class="">Grundzahlen</h2>
				<div v-if="isAdvanced == true">Coming soon</div>
				<div v-else>
					<p>Kaufpreis in €</p>
					<input type="text" placeholder="in €" class="input input-bordered w-full" v-model="buyingPrice" />
					<div class="flex flex-wrap my-4">
						<span class="w-1/2">Kaltmiete in €</span>
						<div class="flex">
							<div class="flex items-center mr-6">
								<input type="radio" name="rent" class="radio radio-accent mr-2" :value="false" v-model="isRentAnnualized" />monatlich
							</div>
							<div class="flex items-center">
								<input type="radio" name="rent" class="radio radio-accent mr-2" :value="true" v-model="isRentAnnualized" />jährlich
							</div>
						</div>
					</div>
					<input type="text" placeholder="in €" class="input input-bordered w-full" v-model="netRent" />
				</div>
			</div>
			<div class="grow w-5/12 min-w-min prose bg-neutral rounded-2xl p-6 m-6">
				<h2>Ergebnisse</h2>
				<div v-if="!isAdvanced">
					<p>Kaufpreis: {{ Intl.NumberFormat('de-DE', { style: 'currency', currency: 'EUR' }).format(buyingPrice) }}</p>
					<p>Kaltmiete pa.: {{ Intl.NumberFormat('de-DE', { style: 'currency', currency: 'EUR' }).format(annualizedNetRent) }}</p>
					<p>Bruttomietrendite: {{ Intl.NumberFormat('de-DE', { style: 'percent', minimumFractionDigits: 2}).format(grossRentalYield) }}</p>
				</div>
				<div v-else>
					<h3 class="-my-4">Grundlegende Zahlen</h3>
					<div class="pl-6">
						<p>Kaufpreis: {{ Intl.NumberFormat('de-DE', { style: 'currency', currency: 'EUR' }).format(buyingPrice) }}</p>
						<p>Kaufnebenkosten: </p>
						<p>Gesamtkosten: </p>
						<p>Kapitalbedarf: </p>
						<p>Kaltmiete pa.: {{ Intl.NumberFormat('de-DE', { style: 'currency', currency: 'EUR' }).format(annualizedNetRent) }}</p>
					</div>
					<h3 class="-mb-4">Renditekennzahlen</h3>
					<div class="pl-6">
						<p>Bruttomietrendite: {{ Intl.NumberFormat('de-DE', { style: 'percent', minimumFractionDigits: 2}).format(grossRentalYield) }}</p>
						<p>freier Cashflow (pa): </p>
						<p>ROI (pa): </p>
					</div>
				</div>
			</div>
		</div>
    </div>
</template>

<style>
	/* * {
		border: 1px solid black;
	} */
</style>

<!-- 
<form>
    Kaufpreis: <input type="text" id="kaufpreis" /> €<br>
    Maklergebühr: <input type="text" id="makler" /> €<br>
    Kaufnebenkosten: <input type="text" id="kaufNk" /> €<br>
    Eigenkapital: <input type="text" id="Ek" /> €<br>
    Zinsen: <input type="text" id="zinsen" /> %<br>
    Tilgung: <input type="text" id="tilgung" /> %<br>
    Nettokaltmiete (pa): <input type="text" id="miete" /> €<br>
    Nicht umlagefähige NK (pa): <input type="text" id="nuNk" /> €<br>
	Rücklagen (pa): <input type="text" id="" /> €<br>
	Instandhaltung (pa): <input type="text" id="" /> €<br>
</form>
function calcRendite() {		
	preisNum = kaufpreisNum + maklerNum + kaufNkNum;
	darlehenNum = preisNum - EkNum;
	renditeNum = mieteNum / kaufpreisNum * 100;
	cashflowNum = mieteNum - nuNkNum - (darlehenNum * zinsenNum / 100) - (darlehenNum * tilgungNum / 100);
	roiNum = (mieteNum - nuNkNum - (darlehenNum * zinsenNum / 100)) / EkNum * 100;
}
-->
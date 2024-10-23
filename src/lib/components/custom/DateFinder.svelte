<script lang="ts">
	import { Label } from '$lib/components/ui/label';
	import { Input } from '$lib/components/ui/input';
	import { cn } from '$lib/utils';

	let year = $state(new Date().getFullYear());
	let dayOfYear = $state(1);
	let errorMessage = $state('');

	function getDateFromDay(year: number, dayOfYear: number): string {
		const date = new Date(year, 0, 1);
		date.setDate(date.getDate() + dayOfYear - 1);
		const day = String(date.getDate()).padStart(2, '0');
		const month = String(date.getMonth() + 1).padStart(2, '0');
		return `${day}/${month}/${year}`;
	}

	function validateDayOfYear() {
		const maxDays = new Date(year, 1, 29).getDate() === 29 ? 366 : 365;
		if (dayOfYear < 1 || dayOfYear > maxDays) {
			errorMessage = `Please enter a valid day of the year (1 - ${maxDays}).`;
		} else {
			errorMessage = '';
		}
	}
</script>

<div class="flex flex-col items-center justify-center min-h-screen bg-gray-100 p-6">
	<h1 class="text-2xl font-semibold mb-4">Date Finder 🔎</h1>
	<div class="flex flex-col space-y-4 w-full max-w-sm">
		<Label class="font-medium">Year:</Label>
		<Input type="number" bind:value={year} placeholder="Enter year" />

		<Label class="font-medium">Day of the Year:</Label>
		<Input
			type="number"
			bind:value={dayOfYear}
			on:input={validateDayOfYear}
			class={cn(errorMessage && 'focus:ring-red-500 focus-visible:ring-red-500')}
			placeholder="Enter day of the year"
			min="1"
			max="366"
		/>
		{#if errorMessage}
			<p class="text-red-500">{errorMessage}</p>
		{/if}
	</div>
	<p class="mt-6 text-lg font-medium text-gray-700">
		Date: {errorMessage ? 'Invalid input' : getDateFromDay(year, dayOfYear)}
	</p>
</div>

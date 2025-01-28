<script>
	import '../app.css';
	import 'iconify-icon';

	let theme = $state('dark');
	function switchTheme() {
		if (theme === 'dark') {
			theme = 'light';
			localStorage.setItem('theme', 'light');
		} else {
			theme = 'dark';
			localStorage.setItem('theme', 'dark');
		}
	}

	$effect(() => {
		if (localStorage.getItem('theme')) {
			theme = localStorage.getItem('theme');
			if (theme === 'light') {
				document.querySelector('.toggle').checked = false;
			}
		}
	});
</script>

<div data-theme={theme} class="min-h-screen p-4">
	<div class="flex justify-end">
		<div class="flex gap-1">
			<iconify-icon icon="lucide-sun" class="text-2xl"></iconify-icon>
			<input
				type="checkbox"
				class="toggle border-none bg-white [--tglbg:#a729f5] hover:bg-white"
				checked="checked"
				onclick={switchTheme}
			/>
			<iconify-icon icon="lucide-moon" class="text-2xl"></iconify-icon>
		</div>
		<!-- {#key DataTransfer.url}
		<div in:fade={{delay:200, duration:200}}out:fade{{duration:200}}>
		{@render children?.()}	
		</div>
		{/key} -->
	</div>
	<slot />
</div>

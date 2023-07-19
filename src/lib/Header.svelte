<script lang="ts">
	import { LightSwitch } from '@skeletonlabs/skeleton';
	 //import ThemeSwitcher from './ThemeSwitcher.svelte';
</script>

<div class="flex justify-between ml-11 ">
	<a href="/" class="unstyled text-black dark:text-slate-200 flex space-x-4 items-center">
		<img class="h-18 md:h-18 w-72" src="/logo-class.svg" alt="classtactics.com" />
		
	</a>

	<div class="flex items-center space-x-2">
		<LightSwitch />

		<!-- See ThemeSwitcher component to find out why this is commented out -->
		 <!--<ThemeSwitcher />-->
	</div>
</div>

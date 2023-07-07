<script lang="ts">
	import { availableMonitors, WebviewWindow, type Monitor } from '@tauri-apps/api/window';
	import { onMount } from 'svelte';

	onMount(async () => {
		const monitors = await availableMonitors(); // get all available monitors

		// The cart monitor is the first monitor that has a non-zero x position
		// As this indicates that the monitor is not the primary monitor

		let cartMonitor: Monitor | undefined = monitors.find((monitor) => monitor.position.x > 0);
		let mainMonitor: Monitor | undefined = monitors.find((monitor) => monitor.position.x === 0);
		console.log(mainMonitor);

		if (!cartMonitor) {
			console.warn('No monitor with x pos greater than 0 detected');
			cartMonitor = monitors[1]; // Fallback to selecting the second monitor
		}

		new WebviewWindow('cart', {
			url: '/cart',
			title: 'Cart',
			height: cartMonitor.size.height,
			width: cartMonitor.size.width,
			x: cartMonitor.size.width,
			y: cartMonitor.position.y
		});
	});
</script>

<slot />

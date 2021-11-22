<script>
	import { quintOut } from 'svelte/easing';
	import { crossfade } from 'svelte/transition';

	const [send, receive] = crossfade({
		duration: d => Math.sqrt(d * 200),

		fallback(node, params) {
			const style = getComputedStyle(node);
			const transform = style.transform === 'none' ? '' : style.transform;

			return {
				duration: 600,
				easing: quintOut,
				css: t => `
					transform: ${transform} scale(${t});
					opacity: ${t}
				`
			};
		}
	});

	let uid = 1;

	let addresses = [
		{ id: uid++, done: false, description: 'iotaajkennnnnnnnnnnnnnnn234323jw' },
		{ id: uid++, done: false, description: 'iotahfuihfief8ef8e78f8zf78sefuf8' },
		{ id: uid++, done: true,  description: 'iotafuheiufhuhbfifjfue76efe8f79f' },
		{ id: uid++, done: false, description: 'iotafuhiufhuihfief8ef9e78fdf9fed' },
		{ id: uid++, done: false, description: 'iotauhfiefewuifhfezhfgehgiefue78' },
		{ id: uid++, done: false, description: 'iotahfudefefhuiue787u8e7f8dfuhfu' },
	];

	function add(input) {
		const address = {
			id: uid++,
			done: false,
			description: input.value
		};

		addresses = [address, ...addresses];
		input.value = '';
	}

	function remove(address) {
		addresses = addresses.filter(t => t !== address);
	}

	
</script>

<div class='board'>
	<input
		placeholder="Add an IOTA Address"
		on:keydown={e => e.key === 'Enter' && add(e.target)}
	>

    <input
		placeholder="Search an IOTA Address"
		on:keydown={e => e.key === 'Enter' && add(e.target)}
	>

	<div class='left'>
		<h2>IOTA Addresses</h2>
		{#each addresses.filter(t => !t.done) as addresses (addresses.id)}
            <ul>
			<li
				in:receive="{{key: addresses.id}}"
				out:send="{{key: addresses.id}}"
			>
				{addresses.description}
				<button on:click="{() => remove(addresses)}">remove</button>
			</li>
        </ul>
		{/each}
	</div>

</div>

<style>
	.board {
		display: grid;
		grid-template-columns: 1fr 1fr;
		grid-gap: 1em;
		max-width: 100em;
		margin: 0 auto;
	}

	.board > input {
		font-size: 1.4em;
		grid-column: 1/3;
	}

	h2 {
		font-size: 2em;
		font-weight: 200;
		user-select: none;
		margin: 0 0 0.5em 0;
	}

	li {
		position: relative;
		line-height: 4;
		padding: 0.5em 2.5em 0.5em 2em;
		margin: 0 0 0.5em 0;
		border-radius: 2px;
		user-select: none;
		border: 1px solid hsl(240, 8%, 70%);
		background-color:hsl(240, 8%, 93%);
		color: #333;
	}
	

	button {
		position: absolute;
		top: 0;
		right: 0.2em;
		width: 2em;
		height: 100%;
		background: no-repeat 50% 50% url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24'%3E%3Cpath fill='%23676778' d='M12,2C17.53,2 22,6.47 22,12C22,17.53 17.53,22 12,22C6.47,22 2,17.53 2,12C2,6.47 6.47,2 12,2M17,7H14.5L13.5,6H10.5L9.5,7H7V9H17V7M9,18H15A1,1 0 0,0 16,17V10H8V17A1,1 0 0,0 9,18Z'%3E%3C/path%3E%3C/svg%3E");
		background-size: 1.4em 1.4em;
		border: none;
		opacity: 0;
		transition: opacity 0.2s;
		text-indent: -9999px;
		cursor: pointer;
	}

	li:hover button {
		opacity: 1;
	}
</style>

<script lang="ts">
	const time = 150;
	const direction = -200;
	const start = {
		delay: time,
		duration: time,
		x: direction
	};
	const stop = {
		duration: time,
		x: direction
	};
	const links = ['', 'admin', 'login', 'logout'];
</script>

<nav>
	<ul>
		<li>
			<a href="/">Eli5</a>
		</li>
		<li>
			<a href="/chat">Chat</a>
		</li>
		<li>
			<a href="/enhance">Enhance</a>
		</li>
	</ul>
</nav>

<style lang="scss">
	nav {
		display: flex;
		justify-content: center;
		align-items: center;
		border-bottom: 0.2rem solid var(--primary);
		margin-bottom: 2rem;

		a {
			margin-right: 1rem;
			font-size: 1.3rem;
		}
	}
</style>

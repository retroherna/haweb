<script lang="ts">
	import { slide } from 'svelte/transition'

    import Loc from '$lib/Loc.svelte';
	import BurgerMenu from '$lib/BurgerMenu.svelte';
	import LanguageSwitcher from './LanguageSwitcher.svelte';

	var links = [
		['/mission', "Mise a cíle", "Missions and goals"],
		['/projects', "Projekty", "Projects"],
		['/services', "Služby a technologie", "Services and technologies"],
		['/collection', "Sbírka", "Collection"],
		['/blog', "Blog", "Blog"],
		['/contact', "Kontakty", "Contacts"],
	];
	let burgerMenuOpen = false;
	let projectsExpanded = false;
</script>

<header>
	<div class="logo">
		<a href="/" on:click={() => projectsExpanded = false}>
			<img src="/ico/logo_herni_archiv.svg" alt="Logo Herního archivu" height=62>
		</a>
	</div>
	<div class="menu">
		<div class="burger-menu">
			<BurgerMenu padding={'25px'} bind:open={burgerMenuOpen}>
				<ul class="burger-links">
					<li>
						<LanguageSwitcher />
					</li>
					{#each links as link}
						<li>
							<a href="{link[0]}" on:click={() => burgerMenuOpen=false}>
								<Loc cs="{link[1]}" en="{link[2]}" />
							</a>
						</li>
					{/each}
				</ul>
			</BurgerMenu>
		</div>
		<div class="language-select">
			<LanguageSwitcher />
		</div>
		<ul class="regular-links">
			{#each links as link}
				<!-- XXX this is a temporary hack -->
				{#if link[0] == '/projects'}
					<li>
						<a
							style="cursor: pointer;"
							on:click={() => projectsExpanded = !projectsExpanded}
						>
							<Loc cs="{link[1]} &nbsp;{projectsExpanded ? '▴' : '▾'}" en="{link[2]} &nbsp;{projectsExpanded ? '▴' : '▾'}" />
						</a>
						{#if projectsExpanded}
							<ul class="dropdown" transition:slide>
								<li>
									<a href="/projects" on:click={() => projectsExpanded = false}>
										<Loc cs="Časová osa" en="Timeline" />
									</a>
								</li>
								<li>
									<a href="https://inventory.retroherna.org/">
										<Loc cs="Inventární systém" en="Inventory system" />
									</a>
								</li>
								<li>
									<a href="https://casopisy.herniarchiv.cz/">
										<Loc cs="Databáze magazínů" en="Magazine database" />
									</a>
								</li>
								<li>
									<a href="/interviews" on:click={() => projectsExpanded = false}>
										<Loc cs="Rozhovory" en="Interviews" />
									</a>
								</li>
								<li>
									<a href="/gallery/emil-fafek" on:click={() => projectsExpanded = false}>
										<Loc cs="Fotografie" en="Photographs" />
									</a>
								</li>
							</ul>
						{/if}
					</li>
				{:else}
					<li>
						<a href="{link[0]}" on:click={() => projectsExpanded = false}>
							<Loc cs="{link[1]}" en="{link[2]}" />
						</a>
					</li>
				{/if}
			{/each}
		</ul>
	</div>
</header>

<style>
	header {
		max-width: var(--max-width);
		margin: auto;
		margin-bottom: 64px;
		padding: 0 16px 0 16px;

		display: flex;
		justify-content: space-between;
		align-content: middle;
	}

	.menu {
		margin: auto 0 auto 0;
	}

	.language-select {
		text-align: right;
		margin-right: 28px;
		position: relative;
		top: -32px;
	}

	ul {
		margin: auto;

		list-style: none;
		display: flex;
		justify-content: end;
		flex-wrap: wrap;
	}

	li {
		padding: 0 27px 0 27px;
		border-right: 1px solid var(--color-secondary);
	}

	.dropdown {
		flex-direction: column;
		position: absolute;
		padding: 0;
		flex-wrap: nowrap;
		margin-top: 4px;
		background-color: var(--color-bg);
		border: 1px solid var(--color-secondary);
		z-index: 50;
		margin-left: -12px;
	}

	.dropdown li {
		padding: 0;
		margin: 8px 16px 8px 12px;
		border: 0;
	}

	li:last-child {
		border-right: none;
	}

	a {
		color: inherit;
		text-transform: uppercase;
		text-decoration: none;
		white-space: nowrap;
	}

	a:hover {
		text-decoration: underline;
	}

	.burger-menu {
		display: none;
	}

	.burger-links {
		flex-direction: column;
		padding-left: 0;
		margin-right: 64px;
	}

	.burger-links li {
		padding-left: 0;
		padding-bottom: 32px;
	}

	.burger-links a {
		white-space: normal;
		font-size: 120%;
	}

	@media screen and (max-width: 1300px) {
		.regular-links > li {
			padding: 0 20px 0 20px;
		}
	}

	@media screen and (max-width: 1200px) {
		.regular-links > li {
			padding: 0 10px 0 10px;
		}
	}

	@media screen and (min-width: 850px) and (max-width: 1100px) {
		.regular-links > li {
			padding: 0 20px 0 20px;
			margin-top: 8px;
			margin-bottom: 8px;
		}
		.regular-links > li:nth-child(3) {
			border-right: 0;
		}
		.regular-links > li:nth-child(4) {
			padding-left: 40%;
		}
	}

	@media screen and (max-width: 850px) {
		.menu ul.regular-links {
			display: none;
		}
		
		.language-select {display: none;}

		.burger-menu {
			display: block;
		}
	}
</style>

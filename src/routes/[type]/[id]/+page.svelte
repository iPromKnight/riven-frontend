<script lang="ts">
	import type { PageData } from './$types';
	import Header from '$lib/components/header.svelte';
	import { Badge } from '$lib/components/ui/badge';
	import { Star, TvMinimalPlay, MonitorDown, ArrowUpRight, Tag } from 'lucide-svelte';
	import { Button } from '$lib/components/ui/button';
	import * as Tooltip from '$lib/components/ui/tooltip';
	import MediaTmdbCarousel from '$lib/components/media-tmdb-carousel.svelte';
	import clsx from 'clsx';

	export let data: PageData;

	let productionCompanies = 4;

	// eslint-disable-next-line @typescript-eslint/no-explicit-any
	function filterSpecial(seasons: any) {
		// eslint-disable-next-line @typescript-eslint/no-explicit-any
		return seasons.filter((season: any) => season.season_number !== 0);
	}
</script>

<svelte:head>
	<title>{data.details.title || data.details.name || data.details.original_name} | Riven</title>
</svelte:head>

<div class="!text-zinc-100">
	<Header />
</div>

<div class="relative flex flex-col text-zinc-100">
	<div class="fixed bottom-0 left-0 z-[-1] h-screen w-full bg-[#09101A]">
		<span>
			<img
				alt={data.details.id}
				class="h-full w-full object-cover opacity-50 blur"
				src="https://www.themoviedb.org/t/p/original{data.details.backdrop_path}"
			/>
			<div
				class="absolute bottom-0 left-0 right-0 h-full w-full bg-gradient-to-b from-transparent to-zinc-900/55"
			></div>
		</span>
	</div>
	<div class="absolute z-[2] mt-32 flex h-full w-full flex-col items-center p-8 md:px-24 lg:px-32">
		<div class="mx-auto flex w-full max-w-7xl flex-col">
			<div class="flex w-full flex-col items-center md:flex-row md:items-start">
				<div class="w-[180px] flex-shrink-0 overflow-hidden md:w-[25%]">
					<div
						class="aspect-[1/1.5] flex-shrink-0 overflow-hidden rounded-lg backdrop-blur md:rounded-xl"
					>
						<img
							alt={data.details.id}
							src="https://www.themoviedb.org/t/p/w780{data.details.poster_path}"
							class="h-full w-full object-cover object-center"
						/>
					</div>
				</div>
				<div class="flex w-full flex-col gap-2 md:w-[75%] md:pl-12 lg:pl-16">
					<h1 class="text-center text-4xl text-zinc-50 md:text-left">
						{data.details.title || data.details.name || data.details.original_name}
					</h1>
					{#if data.details.tagline}
						<h2 class="text-center text-xl italic text-zinc-200 md:text-left">
							&quot;{data.details.tagline}&quot;
						</h2>
					{/if}
					<div class="flex flex-wrap items-center justify-center gap-3 text-base md:justify-start">
						{#if data.details.release_date || data.details.first_air_date}
							<span class="text-zinc-100">
								{data.details.release_date || data.details.first_air_date}
							</span>
						{/if}
						{#if data.details.vote_average}
							<div class="flex items-center gap-1 text-zinc-100">
								<Star class="size-4" />
								<span>{data.details.vote_average}</span>
							</div>
						{/if}
						{#if data.details.original_language}
							<span class="uppercase text-zinc-100">
								{data.details.original_language}
							</span>
						{/if}
						{#if data.details.runtime}
							<span class="uppercase text-zinc-100">
								{data.details.runtime}m
							</span>
						{/if}
					</div>
					<div class="flex flex-wrap items-center justify-center gap-1 md:justify-start">
						{#if data.details.genres}
							{#each data.details.genres as genre}
								<Badge variant="secondary" class="font-medium">{genre.name}</Badge>
							{/each}
						{/if}
					</div>
					<div class="text-center text-base font-thin leading-tight text-zinc-200 md:text-left">
						{data.details.overview}
					</div>
					<div class="mt-4 flex flex-wrap items-center justify-center gap-2 md:justify-start">
						<Button
							href="/404"
							class="flex items-center gap-1 bg-zinc-100 px-6 py-4 text-zinc-900 transition-all duration-200 ease-in-out hover:bg-zinc-200"
						>
							<TvMinimalPlay class="size-4" />
							<span>Watch</span>
						</Button>
						<Button
							href="/404"
							class="flex items-center gap-1 border border-zinc-100 bg-transparent px-6 py-4 hover:bg-transparent/10"
						>
							<MonitorDown class="size-4" />
							<span>Request</span>
						</Button>
					</div>
					{#if data.details.belongs_to_collection}
						<div class="relative mt-4 flex h-full w-full flex-col">
							<div class="z-[-1] w-full">
								<img
									alt={data.details.belongs_to_collection.id}
									src="https://www.themoviedb.org/t/p/w1280{data.details.belongs_to_collection
										.backdrop_path}"
									loading="lazy"
									class="h-full max-h-16 w-full rounded-2xl object-cover object-center"
								/>
							</div>
							<div
								class="absolute inset-0 z-[0] h-full w-full rounded-2xl bg-gradient-to-t from-zinc-900"
							></div>
							<div
								class="absolute inset-0 z-[1] flex items-center justify-between gap-1 p-4 text-zinc-200"
							>
								<h2 class="text-base font-medium">
									{data.details.belongs_to_collection.name}
								</h2>
								<Button
									href="/collection/{data.details.belongs_to_collection.id}"
									variant="link"
									class="text-zinc-200">View Collection</Button
								>
							</div>
						</div>
					{/if}
					{#if data.details.credits && data.details.credits.cast.length > 0}
						<div
							class="mt-2 flex flex-row flex-wrap items-center justify-center gap-2 md:justify-start"
						>
							{#each data.details.credits.cast as cast, i}
								{#if i < 9}
									<Tooltip.Root>
										<Tooltip.Trigger>
											<div class="flex flex-col items-center gap-1">
												<img
													alt={cast.id}
													src={cast.profile_path
														? `https://www.themoviedb.org/t/p/w185${cast.profile_path}`
														: '/images/avatar.png'}
													loading="lazy"
													class="h-16 w-16 rounded-full object-cover object-center"
												/>
											</div>
										</Tooltip.Trigger>
										<Tooltip.Content>
											<p>{cast.name} as</p>
											<p>{cast.character}</p>
										</Tooltip.Content>
									</Tooltip.Root>
								{/if}
							{/each}
							{#if data.details.credits.cast.length > 9}
								<Button
									variant="link"
									href="/credits/{data.details.id}"
									class="flex gap-1 text-zinc-100"
								>
									<ArrowUpRight class="size-4" />
									<span>View All</span>
								</Button>
							{/if}
						</div>
					{/if}
				</div>
			</div>
			<div class="mt-8 flex w-full flex-col items-start gap-8 md:flex-row">
				<div class="flex w-full flex-col items-start md:w-[70%]">
					{#if data.details.credits && data.details.credits.crew.length > 0}
						<div class="grid w-full grid-cols-3 gap-4">
							{#each data.details.credits.crew as crew, i}
								{#if i < 6}
									<div class="flex flex-col gap-1">
										<p class="text-lg font-medium text-zinc-100">{crew.job}</p>
										<p class="text-zinc-200">{crew.name}</p>
									</div>
								{/if}
							{/each}
						</div>
					{/if}
					{#if data.details.keywords}
						{@const keywords = data.details.keywords.keywords || data.details.keywords.results}
						<div class="mt-8 flex w-full flex-wrap gap-2">
							{#each keywords as keyword}
								<Badge class="flex items-center gap-2 bg-secondary/50 font-medium">
									<Tag class="size-4" />
									<span>{keyword.name}</span>
								</Badge>
							{/each}
						</div>
					{/if}
				</div>

				<div class="flex w-full flex-col md:w-[30%]">
					<div class="flex flex-1 flex-col rounded-lg bg-zinc-50/10">
						{#if data.details.status}
							<div
								class="flex items-center justify-between gap-2 p-2 last-of-type:border-none md:p-3"
							>
								<h2 class="text-zinc-100">Status</h2>
								<span class="text-sm text-zinc-300">{data.details.status}</span>
							</div>
						{/if}
						{#if data.details.revenue}
							<div
								class="flex items-center justify-between gap-2 p-2 last-of-type:border-none md:p-3"
							>
								<h2 class="text-zinc-100">Revenue</h2>
								<span class="text-sm text-zinc-300">
									${data.details.revenue.toLocaleString()}
								</span>
							</div>
						{/if}
						{#if data.details.budget}
							<div
								class="flex items-center justify-between gap-2 p-2 last-of-type:border-none md:p-3"
							>
								<h2 class="text-zinc-100">Budget</h2>
								<span class="text-sm text-zinc-300">
									${data.details.budget.toLocaleString()}
								</span>
							</div>
						{/if}
						{#if data.details.production_countries}
							<div
								class={clsx('flex justify-between gap-2 p-2 last-of-type:border-none md:p-3', {
									'items-center': data.details.production_countries.length === 1,
									'items-start': data.details.production_countries.length > 1
								})}
							>
								<h2 class="text-zinc-100">Countries</h2>
								<span class="flex flex-col items-end">
									<ul class="mb-2 space-y-1 text-right text-sm text-zinc-300">
										{#each data.details.production_countries as country}
											<li class="line-clamp-1">{country.name}</li>
										{/each}
									</ul>
								</span>
							</div>
						{/if}
						{#if data.details.production_companies}
							<div
								class={clsx('flex justify-between gap-2 p-3 last-of-type:border-none', {
									'items-center': data.details.production_companies.length === 1,
									'items-start': data.details.production_companies.length > 1
								})}
							>
								<h2 class="text-zinc-100">Companies</h2>
								<span class="flex flex-col items-end">
									<ul class="mb-2 space-y-1 text-right text-sm text-zinc-300">
										{#each data.details.production_companies as company, i}
											{#if i < productionCompanies}
												<li class="line-clamp-1">{company.name}</li>
											{/if}
										{/each}
									</ul>

									{#if data.details.production_companies.length > 4}
										<Button
											on:click={() => {
												productionCompanies =
													productionCompanies < data.details.production_companies.length
														? data.details.production_companies.length
														: 4;
											}}
											variant="link"
											class="text-zinc-200"
										>
											{productionCompanies < data.details.production_companies.length
												? 'View All'
												: 'View Less'}
										</Button>
									{/if}
								</span>
							</div>
						{/if}
					</div>
				</div>
			</div>

			{#if data.details.seasons}
				<div class="mt-16 flex w-full select-none flex-col gap-4 rounded-lg bg-zinc-50/10 p-8">
					<h3 class="text-2xl text-zinc-100">Seasons</h3>

					<div class="relative flex w-full cursor-pointer flex-wrap">
						{#each filterSpecial(data.details.seasons) as season}
							<a
								href="/tv/{data.mediaID}/{season.season_number}"
								class="group relative aspect-[2/1] h-fit w-full p-2 sm:w-1/2 md:w-1/3 lg:w-1/4"
							>
								<div class="h-full w-full overflow-hidden rounded-lg bg-white/10 shadow-xl">
									<img
										alt={season.id}
										src={season.poster_path
											? `https://www.themoviedb.org/t/p/w780${season.poster_path}`
											: 'https://via.placeholder.com/198x228.png?text=No+thumbnail'}
										class=" h-full w-full object-cover brightness-75 transition-all duration-300 ease-in-out group-hover:scale-105"
										loading="lazy"
									/>
									<div class="absolute left-0 top-0 flex h-full w-full flex-col px-4 py-3">
										<div
											class="line-clamp-2 w-fit rounded-md bg-zinc-900/60 px-2 text-sm font-medium capitalize text-white sm:text-base"
										>
											Season {season.season_number}
										</div>
										<div
											class="mt-auto line-clamp-1 self-end rounded-md bg-zinc-900/60 px-2 text-xs text-white sm:text-sm"
										>
											{season.episode_count ? season.episode_count : 'N/A'} episodes
										</div>
									</div>
								</div>
							</a>
						{/each}
					</div>
				</div>
			{/if}

			<div class="mb-32 mt-16 flex w-full select-none flex-col gap-8">
				{#if data.details.recommendations && data.details.recommendations.results.length > 0}
					<MediaTmdbCarousel
						name="Recommendations"
						results={data.details.recommendations.results}
					/>
				{/if}

				{#if data.details.similar && data.details.similar.results.length > 0}
					<MediaTmdbCarousel
						name="Similar titles"
						results={data.details.similar.results}
						mediaType={data.mediaType}
					/>
				{/if}
			</div>
		</div>
	</div>
</div>

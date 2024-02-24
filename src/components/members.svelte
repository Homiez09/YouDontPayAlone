<script lang="ts">
	export let memberCount: string[];
</script>

<div class="flex flex-col">
	{memberCount.length} people{memberCount.length > 1 ? "s" : ""}
	<!-- <div class="avatar-group -space-x-6">
		{#if memberCount.length < 3}
			{#each memberCount as member}
				<div class="avatar">
					<div class="w-8">
						<img src="/images/avatar.png" alt="avatar" />
					</div>
				</div>
			{/each}
		{:else}
			<div class="avatar">
				<div class="w-8">
					<img src="/images/avatar.png" alt="avatar" />
				</div>
			</div>
			<div class="avatar">
				<div class="w-8">
					<img src="/images/avatar.png" alt="avatar" />
				</div>
			</div>
			<div class="avatar placeholder">
				<div class="w-8 bg-neutral-focus text-neutral-content">
					<span>+{memberCount.length - 2}</span>
				</div>
			</div>
		{/if}
	</div> -->
</div>

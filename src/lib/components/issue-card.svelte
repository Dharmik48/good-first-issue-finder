<script lang="ts">
  import type { Node } from '../../global';
  import Card from '$lib/components/card.svelte';
  import Label from '$lib/components/label.svelte';
  import RepoHeader from '$lib/components/repo-header.svelte';

  export let issue: Node;
  let isToggled = true;

  const handleToggle = () => {
    isToggled = !isToggled;
  };
</script>

<Card>
  <div class="flex items-center justify-between">
    <div class="flex min-w-0 items-center">
      <img
        class="mr-4 object-contain dark:hidden"
        src="/images/githubmark-light.svg"
        width="32"
        alt="github"
      />
      <img
        class="mr-4 hidden object-contain dark:block"
        src="/images/githubmark-dark.svg"
        width="32"
        alt="github"
      />
      <div class="truncate leading-3">
        <RepoHeader
          class="mr-1 text-xs md:text-base"
          owner={issue.repository.owner.login}
          repo={issue.repository.name}
          repoLink={issue.repository.url}
        />
        <a
          class="min-w-0 whitespace-normal text-sm font-bold text-skin-text-highlight transition-all duration-200 hover:text-skin-primary active:text-skin-primary md:text-lg md:leading-6"
          href={issue.url}
          target="_blank"
        >
          {issue.title}</a
        >
      </div>
    </div>
    <div class="flex flex-shrink-0">
      <div class="mr-2 flex items-center px-2">
        {#if issue.repository.primaryLanguage}
          <Label
            color={issue.repository.primaryLanguage.color}
            text={issue.repository.primaryLanguage.name}
          />
        {/if}
      </div>
      <a class="hidden sm:block" href={`https://gitpod.io/#${issue.url}`} target="_blank">
        <img class="object-fill" src="/images/open-in-gitpod.svg" alt="open-in-gitpod" /></a
      ><button on:click={() => handleToggle()} class="px-2 md:px-4">
        <img
          src="/images/collapse.svg"
          alt="collapse"
          class="object-fill transition-all duration-200 dark:hidden lg:w-4"
          class:rotate={!isToggled}
        />
        <img
          src="/images/collapse-dark.svg"
          alt="collapse"
          class="hidden object-fill transition-all duration-200 dark:block lg:w-4"
          class:rotate={!isToggled}
        /></button
      >
    </div>
  </div>
  {#if !isToggled}
    <div class="mt-2 space-y-2">
      <span class="flex">
        Created at: {new Date(issue.createdAt)
          .toString()
          .replace(/\S+\s(\S+)\s(\d+)\s(\d+)\s.*/, '$2 $1 $3')}
      </span>
      {#each issue.labels.edges as label}
        <Label text={label.node.name} color={label.node.color} />
      {/each}
    </div>
  {/if}
</Card>

<style lang="postcss">
  .rotate {
    @apply rotate-180;
  }
</style>

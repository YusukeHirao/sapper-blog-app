<script lang="ts">
  import DropDownItem from "./DropDownItem.svelte";
  import Loading from "../Icons/Loading.svelte";
  import type { Item } from "./types";
  import { createEventDispatcher } from "svelte";
  const dispatch = createEventDispatcher<{
    select: Item;
  }>();

  export let items: Item[];
  export let loading = false;
  export let listboxId: string;
  export let optionId: string;
  export let activeIndex: number | null = null;
</script>

<div
  on:keydown
  class="overflow-hiddenrounded-br-md absolute z-20 max-h-96 w-72 overflow-y-scroll rounded-bl-md border-2 border-t-0 border-gray-200 bg-white shadow-lg dark:border-zinc-700 dark:bg-zinc-700"
>
  {#if loading}
    <div class="py-2 text-center" aria-live="polite" aria-atomic="true">
      <div class="sr-only">検索中</div>
      <Loading />
    </div>
  {:else if items.length === 0}
    <div class="py-2 text-center" aria-live="polite" aria-atomic="true">
      <p class="text-gray-500 dark:text-gray-300">検索結果がありません</p>
    </div>
  {:else}
    <ul class="py-2" role="listbox" aria-label="検索結果" id={listboxId}>
      {#each items as item, index (item.key)}
        <DropDownItem
          imageUrl={item.imageUrl}
          text={item.text}
          selected={index === activeIndex}
          id={`${optionId}-${index}`}
          on:mousedown={() => {
            dispatch("select", item);
          }}
        />
      {/each}
    </ul>
  {/if}
</div>

<script lang="ts">
import type { HTMLAttributes } from "svelte/elements"
import type { Chain } from "$lib/schema/chain"
import type { AddressCanonicalBytes } from "$lib/schema/address"
import { cn } from "$lib/utils"
import Tooltip from "$lib/components/ui/Tooltip.svelte"
import Truncate from "$lib/components/ui/Truncate.svelte"
import { Effect, Option } from "effect"

type Props = HTMLAttributes<HTMLDivElement> & {
  address: AddressCanonicalBytes
  chain: Chain
  class?: string
}

const { address, chain, class: className = "", ...rest }: Props = $props()

const displayAddress = $derived(Effect.runSync(chain.getDisplayAddress(address)))
</script>

<Tooltip>
  {#snippet trigger()}
    <div class={cn("font-mono", className)} {...rest}>
      <Truncate value={displayAddress} maxLength={12} showCopy={false}/>
    </div>
  {/snippet}

  {#snippet content()}
    <div class="text-sm flex flex-col gap-4 text-neutral-400">
      <section class="flex justify-between items-center">
        <h2 class="text-white font-bold text-lg">Address Details</h2>
        <div class="bg-sky-400 text-black font-bold rounded px-1">
          {chain.rpc_type.toUpperCase()}
        </div>
      </section>

      <section>
        <h3 class="text-white">Chain</h3>
        <div>{chain.display_name}</div>
        <div class="text-xs">{chain.universal_chain_id}</div>
      </section>

      <section>
        <h3 class="text-white">Formats</h3>
        <div>
          <span class="text-white">Display:</span>
          <span class="font-mono">{displayAddress}</span>
        </div>
        <div>
          <span class="text-white">Canonical:</span>
          <span class="font-mono">{address}</span>
        </div>
      </section>
    </div>
  {/snippet}
</Tooltip>

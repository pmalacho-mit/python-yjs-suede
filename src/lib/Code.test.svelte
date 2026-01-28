<script lang="ts">
  import { Sweater } from "../suede/sweater-vest-suede";
  import { iframe, doc } from "../suede/testyjs-suede/harness";
  import { YFile, YNotebook } from "../../release";

  const { is, src, indexedSrc, index } = iframe();
</script>

<Sweater
  body={async ({ set }) => {
    if (is) {
      const pocket = {
        textarea: null as any as HTMLTextAreaElement,
      };
      set(pocket);
      const { ydoc } = doc({ guid: "test1" });
      const yFile = new YFile({ ydoc });

      const notebook = new YNotebook({
        ydoc,
      });

      notebook.insertCells(0, [
        {
          cell_type: "code",
          source: "print('Hello, world!')",
          metadata: {},
          outputs: [{}],
          execution_count: null,
        },
      ]);
    } else {
      const pocket = {
        iframes: Array<HTMLIFrameElement>(3).fill(null as any),
      };
      set(pocket);
    }
  }}
>
  {#snippet vest(pocket: {
    iframes?: HTMLIFrameElement[];
    textarea?: HTMLTextAreaElement;
  })}
    {#if is}
      <textarea bind:this={pocket.textarea}></textarea>
    {:else if pocket.iframes}
      <div style:height="100%" style:width="100%" style:overflow="hidden">
        <div
          style:display="flex"
          style:flex-direction="row"
          style:align-items="flex-start"
          style:height="100%"
        >
          {#each pocket.iframes as _, i}
            {@const src = indexedSrc(i)}
            <!-- svelte-ignore svelte(a11y_missing_attribute) -->
            <iframe bind:this={pocket.iframes[i]} {src}></iframe>
          {/each}
        </div>
      </div>
    {/if}
  {/snippet}
</Sweater>

<style>
  iframe {
    height: 100%;
    width: 40%;
    margin: 2%;
    border: 1px solid black;
  }
</style>

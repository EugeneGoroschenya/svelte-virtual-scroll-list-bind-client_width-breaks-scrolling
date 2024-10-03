<script>
  import { onDestroy } from 'svelte';
  import VirtualScroll from 'svelte-virtual-scroll-list';

  const random = (min, max) => new Array(Math.floor(min + Math.random() * (max - min))).fill('\u{2500}').join('');
  const generateItems = (count = 100) => new Array(count).fill(0).map((_, i) => ({
    id: i + 1,
    content: random(5, 20)
  }));

  let bindClientWidth = true;
  let items = generateItems();

  let vs;
  let offset = 0;
  let range = {};

  let scrolling;
  const startOrStopScrolling = () => {
    if (scrolling) {
      clearInterval(scrolling);
      scrolling = null;
    } else {
      vs.scrollToBottom();
      scrolling = setInterval(() => {
        if (offset <= 0) {
          clearInterval(scrolling);
          scrolling = null;
        }
        vs.scrollToOffset(offset - 90 * 2);
      }, 200);
    }
  };
  onDestroy(() => clearInterval(scrolling));
</script>

<div class="container">
  <header>
    <input
      type="checkbox"
      id="bindClientWidth"
      bind:checked={bindClientWidth}
      on:change={() => items = generateItems()}
    />
    <label for="bindClientWidth">
      Use 'bind:clientWidth' to break scrolling
      ({bindClientWidth ? 'scrolling DOES NOT work' : 'scrolling works'})
    </label>
  </header>
  <main>
    <div class="vs">
      <VirtualScroll
        data={items}
        let:data
        bind:this={vs}
        on:scroll={({detail}) => {
				  range = detail.range
				  offset = vs.getOffset()
				}}>
        <div class="item">
          <div>
            <div>item #{data.id}</div>
            {#if bindClientWidth}
              <div bind:clientWidth={items[data.id - 1].witdh}>
                {data.content}
              </div>
            {:else}
              {data.content}
            {/if}
            <div>witdh: {data.witdh || '?'}px</div>
          </div>
        </div>
      </VirtualScroll>
    </div>
  </main>
  <footer>
    <div>Range: {JSON.stringify(range)}</div>
    <button on:click={startOrStopScrolling}>
      {scrolling ? 'STOP' : 'Scroll to bottom and start'} scrolling to top
    </button>
  </footer>
</div>

<style>
    .container {
        width: 100%;
        max-width: 720px;
        height: 100%;
        min-height: 270px;
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        margin: auto;
    }

    header {
        display: flex;
        flex-shrink: 0;
        border: 1px solid gray;
        padding: 4px;
    }

    main {
        flex-grow: 1;
        overflow: auto;
        min-height: 2em;
        border-left: 1px solid gray;
        border-right: 1px solid gray;
    }

    footer {
        border: 1px solid gray;
        flex-shrink: 0;
        padding: 4px;
    }

    footer > *, header > * {
        margin: 4px;
    }

    .vs {
        height: 100%;
    }

    .item {
        display: flex;
        justify-content: center;
    }

    .item > div {
        margin: 4px;
        padding: 4px;
        text-align: center;
        border: 1px solid gray;
    }
</style>

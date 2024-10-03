## [svelte-virtual-scroll-list]

[Block-level element bindings] (e.g. `bind:clientWidth`) breaks scrolling.

#### Reproduce with [demo].

Reproduced in Chrome, Opera and Edge (perhaps any Chromium based browser).
It works fine in Firefox and Safari.

![Oct-03-2024 16-19-52-does-not-work](https://github.com/user-attachments/assets/f048719c-271f-407e-ade3-5945781aa6b3)



[svelte-virtual-scroll-list]: https://github.com/v1ack/svelte-virtual-scroll-list
[Block-level element bindings]: https://svelte.dev/docs/element-directives#block-level-element-bindings
[demo]: https://eugenegoroschenya.github.io/svelte-virtual-scroll-list-bind-client_width-breaks-scrolling/

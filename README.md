# sveltekit-example-loading-data

[![Open in StackBlitz](https://developer.stackblitz.com/img/open_in_stackblitz.svg)](https://stackblitz.com/github/maiertech/sveltekit-example-loading-data?file=src/routes/index.svelte)

In this example I explore different ways of [loading data from an external API in SvelteKit](https://kit.svelte.dev/docs/loading).

## Load data client-side with `onMount`

`src/routes/load-in-onmount.svelte`

This is the Svelte way of loading data. `<script>` runs when a component instance is created. `onMount` schedules a callback which fires when the component has been mounted to the DOM. Only then will data be loaded. The data will load every time the component is mounted, which for page components means every time you navigate to the page.

When SvelteKit renders the page server-side, the `#await` block renders as `undefined`, because `promise` is `undefined`. Once the browser has loaded the JavaScript and hydrated the page, it shows the loading state and loads data. When loading data is complete, the page renders the data.

The loading state is visible on slow connections, it flashes on fast connections.

## Load data with SvelteKit `load` function

`src/routes/sveltekit-load-function.svelte`

In SvelteKit, page and layout components in `src/routes` can export a `load` function in `<script context="module">`, which SvelteKit runs before the component mounts. SvelteKit ignores `load` for any other components. `load` can run server-side and client-side. Within `load` you have to use the provided `fetch`. This is an optimized fetch, which ensures data is not loaded client-side when it has already been loaded server-side.

A page (or layout) will mount only when `load` has completed, i.e. all promises have been resolved. If it takes long to load data, you will experience a delay when clicking a link to that page. Whether `load` runs server-side or client-side depends on what you do:

- When you load the page directly (or reload), `load` runs server-side.
- When you navigate to the page from within the hydrated app, `load` runs client-side.

### Load data with SvelteKit `load` function and prerender

`src/routes/sveltekit-load-function-prerender.svelte`

Same as before, byt we set

```js
export const prerender = true;
```

in `<script context="module">`. This signals to SvelteKit that this page will display the same content to all users. SvelteKit prerenders route `/sveltekit-load-function-prerender`, but it still runs `load` like before. I do not notice a performance difference.

### Load data in page endpoint

DESCRIBE

Do the same thing with a page endpoint and show that it creates load function implicitly.

## Delegate loading to page

## Loading spinner

You can implement a loading spinner as shown in this video: https://www.youtube.com/watch?v=4Mp-EQnJSTw. The spinner is then visible on the page on which you clicked the link.

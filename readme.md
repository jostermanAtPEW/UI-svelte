A few reusable components built in Svelte. To be instantiated using Svelte's [client-side component API](https://svelte.dev/docs#run-time-client-side-component-api).

For example:

```javascript
import App from './App.svelte';

const app = new App({
	target: document.body,
	props: {
		// assuming App.svelte contains something like
		// `export let answer`:
		answer: 42
	}
});
```
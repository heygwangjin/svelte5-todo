# Svelte 5

## Event handlers

Svelte 5 에서는 더 이상 `on:` directive를 사용하여 이벤트 리스너를 등록하지 않고, 다른 프로퍼티처럼 작성하면 된다.

```html
<script>
	let count = $state(0);
</script>

<button onclick="{()" ="">count++}> clicks: {count}</button>
```
## Developing

Once you've created a project and installed dependencies with `npm install` (or `pnpm install` or `yarn`), start a development server:

```bash
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of your app:

```bash
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy your app, you may need to install an [adapter](https://kit.svelte.dev/docs/adapters) for your target environment.

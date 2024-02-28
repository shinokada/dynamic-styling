<script lang="ts">
  const stylesImport = import.meta.glob('$lib/styles/*.css');
  // console.log(stylesImport);
  let selected = $state('light');
  const styles = Object.entries(stylesImport).map(([path, importFn]) => ({
    value: path.replace('/src/lib/styles/', '').replace('.css', ''),
    name: path.slice(path.lastIndexOf('/') + 1, -4)
  }));
  // console.log(styles);
  $effect(() => {
    let link: HTMLLinkElement;
    (async () => {
      const css = await import(`../lib/styles/${selected}.css?url`);
      link = document.createElement('link')

      link.rel = 'stylesheet';
      link.href = css.default;
      document.head.append(link)
    })();

    return () => {
      // clean up
      link.remove()
    };
  });
</script>

<div class="w-64">
  <label for="html">Select a theme</label><br>
    <select class="mt-2 p-2 border border-gray-200" bind:value={selected}>
    {#each styles as theme}
    <option value={theme.value}>{theme.value}</option>
    {/each}
    </select>
</div>

<h1>This is a heading</h1>
<p>This is some paragraph text. You can dynamically change the styles of various elements in this page.</p>
<a href="/">This is a link</a>
<br>
<button>Click me!</button>

<script>
  import { Canvas } from '@threlte/core'
  import Scene from './Scene.svelte'
</script>

<Canvas>
  <Scene />
</Canvas>

<style>
  :global(html),
  :global(body) {
    width: 100%;
    height: 100%;
    margin: 0;
  }

  :global(body) {
    background: #101522;
  }
</style>

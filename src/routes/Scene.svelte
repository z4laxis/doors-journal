<script>
  import { T } from '@threlte/core'
  import { GLTFLoader } from 'three/examples/jsm/loaders/GLTFLoader.js'
  import { useLoader } from '@threlte/core'
  import { AmbientLight, Box3, DirectionalLight, PerspectiveCamera, Vector3 } from 'three'
  import modelUrl from '#lib/assets/models/journal.gltf?url'

  const gltf = useLoader(GLTFLoader).load(modelUrl)

  const materialColors = {
    Coverfront1Mtl: '#3b2d28',
    Coverfront2Mtl: '#3b2d28',
    Papercover1Mtl: '#3b2d28',
    Papercover2Mtl: '#3b2d28',
    Pagefrontanimated1Mtl: '#3b2d28',
    Pagefrontanimated2Mtl: '#e5d2aa',
    Cornertab1Mtl: '#3b2d28',
    Cornertab2Mtl: '#3b2d28',
    Spinefront1Mtl: '#4a241b',
    Connector1Mtl: '#41322e',
    Tape1Mtl: '#3b2d28',
    Golddot1Mtl: '#665344',
    Rungsbase1Mtl: '#2b2421'
  }

  function styleModel(object) {
    object.position.set(0, 0, 0)
    object.scale.setScalar(0.5)
    object.updateWorldMatrix(true, true)

    const bounds = new Box3().setFromObject(object)
    const center = bounds.getCenter(new Vector3())
    object.position.sub(center)

    object.traverse((child) => {
      if (!child.isMesh) return

      const materials = Array.isArray(child.material) ? child.material : [child.material]
      materials.forEach((material) => {
        const color = materialColors[material.name]
        if (color) material.color.set(color)
        material.needsUpdate = true
      })
    })
  }
</script>

{#if $gltf}
  <T is={$gltf.scene} oncreate={styleModel} />
  <T
    is={PerspectiveCamera}
    position={[0, 0.5, 2.5]}
    makeDefault
    oncreate={(camera) => camera.lookAt(0, 0, 0)}
  />
    <T is={AmbientLight} args={[0xffffff, 1.5]} />
  <T is={DirectionalLight} args={[0xffffff, 2]} position={[4, 6, 8]} />
{/if}
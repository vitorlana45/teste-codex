<script context="module" lang="ts">
  export type PieceType = 'pawn' | 'rook' | 'knight' | 'bishop' | 'queen' | 'king';
</script>

<script lang="ts">
  import { T } from '@threlte/core';

  export let type: PieceType;
  export let color: 'white' | 'black';

  const materialColor = color === 'white' ? '#ffffff' : '#000000';
</script>

<T.Group>
  {#if type === 'pawn'}
    <T.Mesh>
      <T.CylinderGeometry args={[0.35, 0.4, 0.4, 32]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
    <T.Mesh position={[0, 0.4, 0]}>
      <T.SphereGeometry args={[0.25, 32, 32]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
  {:else if type === 'rook'}
    <T.Mesh>
      <T.CylinderGeometry args={[0.4, 0.45, 0.6, 32]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
    <T.Mesh position={[0, 0.3, 0]}>
      <T.BoxGeometry args={[0.5, 0.2, 0.5]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
  {:else if type === 'knight'}
    <T.Mesh>
      <T.CylinderGeometry args={[0.4, 0.45, 0.6, 32]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
    <T.Mesh position={[0, 0.6, 0]} rotation={[Math.PI / 2, 0, 0]}>
      <T.ConeGeometry args={[0.35, 0.6, 32]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
  {:else if type === 'bishop'}
    <T.Mesh>
      <T.CylinderGeometry args={[0.35, 0.4, 0.6, 32]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
    <T.Mesh position={[0, 0.6, 0]}>
      <T.ConeGeometry args={[0.3, 0.5, 32]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
  {:else if type === 'queen'}
    <T.Mesh>
      <T.CylinderGeometry args={[0.4, 0.45, 0.8, 32]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
    <T.Mesh position={[0, 0.8, 0]}>
      <T.SphereGeometry args={[0.25, 32, 32]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
  {:else}
    <T.Mesh>
      <T.CylinderGeometry args={[0.4, 0.45, 0.8, 32]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
    <T.Mesh position={[0, 0.8, 0]} rotation={[0, 0, Math.PI / 4]}>
      <T.BoxGeometry args={[0.05, 0.5, 0.05]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
    <T.Mesh position={[0, 0.8, 0]} rotation={[0, 0, 0]}>
      <T.BoxGeometry args={[0.5, 0.05, 0.05]} />
      <T.MeshStandardMaterial color={materialColor} />
    </T.Mesh>
  {/if}
</T.Group>

<script lang="ts">
  import { Canvas, T } from '@threlte/core';
  import { OrbitControls } from '@threlte/extras';

  type Color = 'white' | 'black';
  interface Piece {
    id: number;
    color: Color;
    x: number;
    y: number;
  }

  const boardSize = 8;
  const squareSize = 1;

  let pieces: Piece[] = [
    // pawns
    ...Array.from({ length: 8 }, (__, i) => ({ id: i + 1, color: 'white' as const, x: i, y: 1 })),
    ...Array.from({ length: 8 }, (__, i) => ({ id: i + 9, color: 'black' as const, x: i, y: 6 }))
  ];

  let selected: Piece | null = null;

  function handlePieceClick(piece: Piece) {
    selected = piece;
  }

  function handleSquareClick(x: number, y: number) {
    if (selected) {
      pieces = pieces.map((p) => (p.id === selected!.id ? { ...p, x, y } : p));
      selected = null;
    }
  }
</script>

<div class="scene">
<Canvas>
  <T.AmbientLight intensity={0.5} />
  <T.DirectionalLight position={[4,5,2]} />

  {#each Array(boardSize) as _, i}
    {#each Array(boardSize) as _, j}
      <T.Mesh
        position={[i - boardSize / 2 + 0.5, 0, j - boardSize / 2 + 0.5]}
        on:click={() => handleSquareClick(i, j)}>
        <T.BoxGeometry args={[squareSize, 0.1, squareSize]} />
        <T.MeshStandardMaterial color={(i + j) % 2 === 0 ? '#eeeeee' : '#444444'} />
      </T.Mesh>
    {/each}
  {/each}

  {#each pieces as p (p.id)}
    <T.Mesh
      position={[p.x - boardSize / 2 + 0.5, 0.5, p.y - boardSize / 2 + 0.5]}
      on:click={() => handlePieceClick(p)}>
      <T.CylinderGeometry args={[0.3, 0.3, 1, 32]} />
      <T.MeshStandardMaterial color={p.color === 'white' ? '#ffffff' : '#000000'} />
    </T.Mesh>
  {/each}

  <OrbitControls />
</Canvas>
</div>

<style>
  .scene {
    width: 100%;
    height: 100vh;
  }
  :global(canvas) {
    touch-action: none;
  }
</style>

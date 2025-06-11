<script lang="ts">
  import { Canvas, T } from '@threlte/core';
  import { OrbitControls } from '@threlte/extras';

  import PieceComponent, { type PieceType } from './Piece.svelte';

  type Color = 'white' | 'black';
  interface Piece {
    id: number;
    type: PieceType;
    color: Color;
    x: number;
    y: number;
  }

  const boardSize = 8;
  const squareSize = 1;

  let pieces: Piece[] = [
    // white pieces
    { id: 1, type: 'rook', color: 'white', x: 0, y: 0 },
    { id: 2, type: 'knight', color: 'white', x: 1, y: 0 },
    { id: 3, type: 'bishop', color: 'white', x: 2, y: 0 },
    { id: 4, type: 'queen', color: 'white', x: 3, y: 0 },
    { id: 5, type: 'king', color: 'white', x: 4, y: 0 },
    { id: 6, type: 'bishop', color: 'white', x: 5, y: 0 },
    { id: 7, type: 'knight', color: 'white', x: 6, y: 0 },
    { id: 8, type: 'rook', color: 'white', x: 7, y: 0 },
    ...Array.from({ length: 8 }, (_, i) => ({
      id: 9 + i,
      type: 'pawn' as const,
      color: 'white' as const,
      x: i,
      y: 1
    })),
    // black pieces
    { id: 17, type: 'rook', color: 'black', x: 0, y: 7 },
    { id: 18, type: 'knight', color: 'black', x: 1, y: 7 },
    { id: 19, type: 'bishop', color: 'black', x: 2, y: 7 },
    { id: 20, type: 'queen', color: 'black', x: 3, y: 7 },
    { id: 21, type: 'king', color: 'black', x: 4, y: 7 },
    { id: 22, type: 'bishop', color: 'black', x: 5, y: 7 },
    { id: 23, type: 'knight', color: 'black', x: 6, y: 7 },
    { id: 24, type: 'rook', color: 'black', x: 7, y: 7 },
    ...Array.from({ length: 8 }, (_, i) => ({
      id: 25 + i,
      type: 'pawn' as const,
      color: 'black' as const,
      x: i,
      y: 6
    }))
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
    <T.Group
      position={[p.x - boardSize / 2 + 0.5, 0, p.y - boardSize / 2 + 0.5]}
      on:click={() => handlePieceClick(p)}>
      <PieceComponent type={p.type} color={p.color} />
    </T.Group>
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

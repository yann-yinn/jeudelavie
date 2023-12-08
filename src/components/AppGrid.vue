<template>
  <div class="game-container">
    <div class="info">
      Génération: {{ gameState.generation }}
      <button @click="nextStep">Prochaine Étape</button>
      <div>
        <button @click="initializeRandom">Générer Grille Aléatoire</button>
      </div>
      <div>
        <button @click="initializeWithConfiguration('planeur')">Planeur</button>
        <button @click="initializeWithConfiguration('ruche')">Ruche (stable)</button>
        <button @click="initializeWithConfiguration('blinker')">Blinker</button>
        <button @click="initializeWithConfiguration('toad')">Toad</button>
        <button @click="initializeWithConfiguration('bateau')">Bateau (stable)</button>
        <button @click="initializeWithConfiguration('sapinDeNoel')">Sapin de Noël</button>
        <button @click="initializeWithConfiguration('pulsar')">Pulsar</button>
        <button @click="initializeWithConfiguration('pentadecathlon')">Pentadecathlon</button>
        <button @click="initializeWithConfiguration('planeurCote')">Planeur par le Côté</button>
      </div>
    </div>

    <div
      class="grid"
      :style="{ gridTemplateColumns: `repeat(${gameState.grilleActuelle[0].length}, 20px)` }"
    >
      <div v-for="(row, i) in gameState.grilleActuelle" :key="i" class="row">
        <div v-for="(cell, j) in row" :key="j" class="cell" :class="{ alive: cell === 1 }"></div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const props = defineProps({
  rows: {
    type: Number,
    required: true
  },
  columns: {
    type: Number,
    required: true
  }
})

type Cellule = 0 | 1
type Grille = Cellule[][]

class GameState {
  grilleActuelle: Grille
  grilleSecondaire: Grille
  generation: number

  constructor(grilleActuelle: Grille) {
    this.grilleActuelle = grilleActuelle
    this.grilleSecondaire = this.cloneAndReset(grilleActuelle)
    this.generation = 0
  }

  cloneAndReset(grille: Grille): Grille {
    return grille.map((ligne) => ligne.map(() => 0))
  }

  calculerProchainEtat() {
    for (let i = 0; i < this.grilleActuelle.length; i++) {
      for (let j = 0; j < this.grilleActuelle[i].length; j++) {
        const voisinsVivants = this.compterVoisins(i, j)
        const celluleActuelle = this.grilleActuelle[i][j]
        if (celluleActuelle === 1 && (voisinsVivants < 2 || voisinsVivants > 3)) {
          this.grilleSecondaire[i][j] = 0
        } else if (celluleActuelle === 0 && voisinsVivants === 3) {
          this.grilleSecondaire[i][j] = 1
        } else {
          this.grilleSecondaire[i][j] = celluleActuelle
        }
      }
    }
    ;[this.grilleActuelle, this.grilleSecondaire] = [
      this.grilleSecondaire,
      this.cloneAndReset(this.grilleActuelle)
    ]
    this.generation++
  }

  compterVoisins(ligne: number, colonne: number): number {
    let compteur = 0
    for (let i = -1; i <= 1; i++) {
      for (let j = -1; j <= 1; j++) {
        if (i === 0 && j === 0) continue // Ignorer la cellule elle-même
        const x = ligne + i
        const y = colonne + j
        if (
          x >= 0 &&
          x < this.grilleActuelle.length &&
          y >= 0 &&
          y < this.grilleActuelle[x].length
        ) {
          compteur += this.grilleActuelle[x][y]
        }
      }
    }
    return compteur
  }
}

// Fonction pour créer une grille vide
function grilleVide(lignes: number, colonnes: number): Grille {
  return Array.from({ length: lignes }, () => Array.from({ length: colonnes }, () => 0 as Cellule))
}

// 1. Planeur (Glider)
function planeur(lignes: number, colonnes: number): Grille {
  const grille = grilleVide(lignes, colonnes)
  grille[1][2] = 1
  grille[2][3] = 1
  grille[3][1] = 1
  grille[3][2] = 1
  grille[3][3] = 1
  return grille
}

// 2. Ruche (Beehive)
function ruche(lignes: number, colonnes: number): Grille {
  const grille = grilleVide(lignes, colonnes)
  grille[2][2] = 1
  grille[2][3] = 1
  grille[3][1] = 1
  grille[3][4] = 1
  grille[4][2] = 1
  grille[4][3] = 1
  return grille
}

// 3. Blinker
function blinker(lignes: number, colonnes: number): Grille {
  const grille = grilleVide(lignes, colonnes)
  grille[2][1] = 1
  grille[2][2] = 1
  grille[2][3] = 1
  return grille
}

// 4. Toad
function toad(lignes: number, colonnes: number): Grille {
  const grille = grilleVide(lignes, colonnes)
  grille[2][2] = 1
  grille[2][3] = 1
  grille[2][4] = 1
  grille[3][1] = 1
  grille[3][2] = 1
  grille[3][3] = 1
  return grille
}

// 5. Bateau (Boat)
function bateau(lignes: number, colonnes: number): Grille {
  const grille = grilleVide(lignes, colonnes)
  grille[1][1] = 1
  grille[1][2] = 1
  grille[2][1] = 1
  grille[2][3] = 1
  grille[3][2] = 1
  return grille
}

// 6. Sapin de Noël (Christmas Tree)
function sapinDeNoel(lignes: number, colonnes: number): Grille {
  const grille = grilleVide(lignes, colonnes)
  grille[3][3] = 1
  grille[4][2] = 1
  grille[4][4] = 1
  grille[5][1] = 1
  grille[5][3] = 1
  grille[5][5] = 1
  grille[6][2] = 1
  grille[6][4] = 1
  grille[7][3] = 1
  return grille
}

function pulsar(lignes: number, colonnes: number): Grille {
  const grille = grilleVide(lignes, colonnes)
  // Placez les cellules vivantes pour créer un Pulsar
  // Les positions sont ajustées pour une grille d'au moins 17x17
  const positions = [
    [2, 4],
    [2, 5],
    [2, 6],
    [2, 10],
    [2, 11],
    [2, 12],
    [4, 2],
    [4, 7],
    [4, 9],
    [4, 14],
    [5, 2],
    [5, 7],
    [5, 9],
    [5, 14],
    [6, 2],
    [6, 7],
    [6, 9],
    [6, 14],
    [7, 4],
    [7, 5],
    [7, 6],
    [7, 10],
    [7, 11],
    [7, 12],
    [9, 4],
    [9, 5],
    [9, 6],
    [9, 10],
    [9, 11],
    [9, 12],
    [10, 2],
    [10, 7],
    [10, 9],
    [10, 14],
    [11, 2],
    [11, 7],
    [11, 9],
    [11, 14],
    [12, 2],
    [12, 7],
    [12, 9],
    [12, 14],
    [14, 4],
    [14, 5],
    [14, 6],
    [14, 10],
    [14, 11],
    [14, 12]
  ]
  positions.forEach(([x, y]) => {
    if (x < lignes && y < colonnes) {
      grille[x][y] = 1
    }
  })
  return grille
}

function pentadecathlon(lignes: number, colonnes: number): Grille {
  const grille = grilleVide(lignes, colonnes)
  // Placez les cellules vivantes pour créer un Pentadecathlon
  // Position ajustée pour une grille d'au moins 10x18
  const startX = Math.floor(lignes / 2) - 1
  const startY = Math.floor(colonnes / 2) - 5
  for (let i = 0; i < 10; i++) {
    if (i === 1 || i === 8) {
      grille[startX - 1][startY + i] = 1
      grille[startX + 1][startY + i] = 1
    } else {
      grille[startX][startY + i] = 1
    }
  }
  return grille
}

function planeurCote(lignes: number, colonnes: number): Grille {
  const grille = grilleVide(lignes, colonnes)
  // Ajouter un planeur sur le côté de la grille
  const startX = 1
  const startY = 1
  grille[startX][startY + 1] = 1
  grille[startX + 1][startY + 2] = 1
  grille[startX + 2][startY] = 1
  grille[startX + 2][startY + 1] = 1
  grille[startX + 2][startY + 2] = 1
  return grille
}

const initializeRandom = () => {
  gameState.value = new GameState(genererGrilleAleatoire(props.rows, props.columns))
}

function genererGrilleAleatoire(lignes: number, colonnes: number): Grille {
  const grille: Grille = []
  for (let i = 0; i < lignes; i++) {
    const ligne: Cellule[] = []
    for (let j = 0; j < colonnes; j++) {
      ligne.push(Math.random() < 0.5 ? 0 : 1)
    }
    grille.push(ligne)
  }
  return grille
}

// Fonction pour initialiser la grille avec une configuration spécifique
const initializeWithConfiguration = (configName: string) => {
  let nouvelleGrille: Grille

  switch (configName) {
    case 'planeur':
      nouvelleGrille = planeur(props.rows, props.columns)
      break
    case 'ruche':
      nouvelleGrille = ruche(props.rows, props.columns)
      break
    case 'blinker':
      nouvelleGrille = blinker(props.rows, props.columns)
      break
    case 'toad':
      nouvelleGrille = toad(props.rows, props.columns)
      break
    case 'bateau':
      nouvelleGrille = bateau(props.rows, props.columns)
      break
    case 'sapinDeNoel':
      nouvelleGrille = sapinDeNoel(props.rows, props.columns)
      break
    case 'pulsar':
      nouvelleGrille = pulsar(props.rows, props.columns)
      break
    case 'pentadecathlon':
      nouvelleGrille = pentadecathlon(props.rows, props.columns)
      break
    case 'planeurCote':
      nouvelleGrille = planeurCote(props.rows, props.columns)
      break
    default:
      nouvelleGrille = grilleVide(props.rows, props.columns) // ou une autre configuration par défaut
  }

  gameState.value = new GameState(nouvelleGrille)
}

const gameState = ref(new GameState(planeur(props.rows, props.columns)))

const nextStep = () => {
  gameState.value.calculerProchainEtat()
}
</script>

<style>
.grid {
  display: grid;
  gap: 1px;
}
.cell {
  width: 20px;
  height: 20px;
  background-color: #eee;
}
.cell.alive {
  background-color: black;
}
</style>

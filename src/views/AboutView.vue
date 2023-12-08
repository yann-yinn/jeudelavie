<template>
  <div class="about">
    <h1>Le jeu de la vie</h1>
    <p>
      Le Jeu de la Vie, aussi connu sous le nom de "Life", est un automate cellulaire développé par
      le mathématicien britannique John Horton Conway en 1970. Il s'agit d'un jeu à zéro joueur, ce
      qui signifie que son évolution est déterminée par son état initial, sans aucune intervention
      ultérieure de la part de l'utilisateur après le démarrage. Le jeu se déroule sur une grille
      infinie bidimensionnelle de cellules, chacune se trouvant dans l'un des deux états possibles :
      vivante ou morte. Chaque cellule interagit avec ses huit voisins, qui sont les cellules
      adjacentes horizontalement, verticalement et en diagonale. À chaque étape, les transitions
      suivent des règles simples : Une cellule vivante qui a moins de deux voisins vivants meurt,
      simulant une sous-population. Une cellule vivante avec deux ou trois voisins vivants continue
      de vivre à la génération suivante. Une cellule vivante avec plus de trois voisins vivants
      meurt, simulant la surpopulation. Une cellule morte avec exactement trois voisins vivants
      devient une cellule vivante, simulant la reproduction. Ces règles simples conduisent à un
      grand nombre de configurations et comportements, allant de structures stables à des
      oscillateurs et des motifs qui semblent se déplacer à travers la grille. Le Jeu de la Vie est
      célèbre pour sa capacité à simuler les processus de naissance, de mort et de survie, et a été
      largement étudié en informatique théorique, en mathématiques et en biologie théorique pour son
      pouvoir de modéliser des systèmes complexes à partir de règles simples.
    </p>
  </div>
</template>

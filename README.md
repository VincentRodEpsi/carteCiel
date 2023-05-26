<div id="top"></div>
<br />

<div align="center">
<a href="https://github.com/VincentRodEpsi/carteCiel">
  <p style="font-size: 5em">🌠</p>
</a>

<h3 align="center">Carte du ciel</h3>

![GitHub package.json version](https://img.shields.io/github/package-json/v/VincentRodEpsi/carteCiel?label=Version)
&nbsp;
![GitHub](https://img.shields.io/github/license/VincentRodEpsi/carteCiel?label=Licence)

  <p align="center">
    Projet de carte du ciel du cours de Performance du SI.
    <br />
    <a href="https://skymap.yanis-petit.fr/"><strong>skymap.yanis-petit.fr »</strong></a>
  </p>
</div>
<br />

<details>
  <summary>Sommaire</summary>
  <ol>
    <li>
      <a href="#à-propos">A propos</a>
      <ul>
        <li><a href="#fonctionnalités">Fonctionnalités</a></li>
        <li><a href="#technologies">Technologies</a></li>
      </ul>
    </li>
    <li>
      <a href="#pour-commencer">Pour commencer</a>
      <ul>
        <li><a href="#prérequis">Prérequis</a></li>
        <li><a href="#développement">Développement</a></li>
        <li><a href="#compilation">Compilation</a></li>
        <li><a href="#deploy">Deploy</a></li>
      </ul>
    </li>
    <li><a href="#auteur">Auteur</a></li>
  </ol>
</details>

## À propos

Projet de carte du ciel interactive sur navigateur du cours de Performance du SI.

### Fonctionnalités

- Affiche les étoiles visibles dans le ciel
- Affiche les détails d'une étoile au clic
- Permet à l'utilisateur de :
    - Choisir le nombre d'étoiles les plus proches à afficher
    - Choisir le nombre d'étoiles les plus chaudes à afficher
    - Choisir le nombre d'étoiles les plus brillantes à afficher
    - Rechercher une étoile par son nom

### Technologies

- [VueJS](https://vuejs.org/)
- [ThreeJS](https://threejs.org/)

<p align="right">(<a href="#top">retour en haut</a>)</p>

## Pour commencer

### Prérequis

- Installer [NodeJS](https://nodejs.org/) avec [npm](https://www.npmjs.com/)

```bash
  curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
  sudo apt-get install -y nodejs
```

### Développement

- Installer les dépendances

````bash
  npm install
````

- Correction des fichiers

````bash
npm run lint
````

- Lancer le serveur de développement

````bash
  npm run dev 
````

- Le serveur est lancé sur l'adresse [http://localhost:8080](http://localhost:8080).

### Compilation

- Compiler le projet

````bash
  npm run build
````

- Le projet est compilé dans le dossier `dist` prêt à être déployé sur un serveur
  web ([Apache](https://httpd.apache.org/), [Nginx](https://www.nginx.com/), etc.).

<p align="right">(<a href="#top">retour en haut</a>)</p>

## Auteur

[@Minarox](https://www.github.com/Minarox)

<p align="right">(<a href="#top">back to top</a>)</p>

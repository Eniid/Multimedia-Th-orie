# Multimédia Théorie

Petit tutoriel pour arriver à faire facilement un peut prés tout se qu'on à vu au cours de multimédia. 



## Les bases

- [ ] Crée le riposte git

- [ ] npm init -y

- [ ] npm install -D laravel-mix

- [ ] npm install -D cross-env

- [ ] modifier le package.json (voir laravel-mix doc)

  ```javascript
  "scripts": {
      "dev": "npm run development",
      "development": "cross-env NODE_ENV=development node_modules/webpack/bin/webpack.js --progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js",
      "watch": "npm run development -- --watch",
      "hot": "cross-env NODE_ENV=development node_modules/webpack-dev-server/bin/webpack-dev-server.js --inline --hot --config=node_modules/laravel-mix/setup/webpack.config.js",
      "prod": "npm run production",
      "production": "cross-env NODE_ENV=production node_modules/webpack/bin/webpack.js --no-progress --hide-modules --config=node_modules/laravel-mix/setup/webpack.config.js"
  }
  ```

- [ ] ajouter un `.gitignore` avec le node-module

  ```javascript
  /node_modules
  ```

- [ ] Crée l'architecture (avec webpack.mix.js)

- [ ] Mettre sur gitHub

  

## HTML // CSS

- [ ] Dans le fichier HTML lier le CSS

  ```html
      <link rel="stylesheet" type="text/css" href="dist/app.css"  />
  ```

- [ ] Tjs dans le fichier HTML lier le JavaScript 

  ```html
      <script type="text/javascript" src="dist/app.js"></script>
  ```

- [ ] Dans le SCSS définire les petit choses de base 

  ```css
  /* Recette css tout basque */
  * {
      padding: 0;
      margin: 0;
  }
  
  
  body {
      overflow : hidden; /* A ajouter principalement si le canvas fait 100% */
  }
  
  
  canvas {
      /* Définir la couleur de font, les bordures du canvas et tout ça */
  }
  
  
  ```





## BASES D'UN FICHER DE JEU 

### Création du fichier de base

- [ ] Crée l'object annimation 

- [ ] Ajouter les variables

  - [ ] CanvasElt : undefinde, (sera défini dans l'init)

  - [ ] ctx : undefind, ("")

    Tableau, objects stoquer ici et tout autre variable utile y seront ajouter par la suite

- [ ] Crée un init(). 

  - [ ] Y définire le canvas

  - [ ] y définire le contexte 

    crée des nouveau objects a partire des classes qui seront crée par la suite 

    appeller le contrôleur 

- [ ] Crée un draw(). 

  Il sera vide au départ, serivra à utiliser uptate une classe, appellé ensuite dans animate pour avoir l'annimation l'objet. 

- [ ] crée animate() 

  - [ ] copy / paste le petit truc de l'annimation

    ``````javascript
    this.ctx.clearRect(0, 0, this.canvasElt.width, this.canvasElt.height);
            this.draw()
            window.requestAnimationFrame( ()=> {
                this.animate(); 
            });
    ``````

- [ ] Si le canvas fait 100 % ajouter dans le init

  ```javascript
  this.resize(); //Donne la taille de 100% à la page
  window.addEventListener('resize', e => {
              this.resize(); // Redonne la taille de 100% à chaque fois
   }, false)
  ```

- [ ] après le animate : 

  ```javascript
      resize(){
          this.canvasElt.height = window.innerHeight;
          this.canvasElt.width = window.innerWidth;
      }
  ```

  - [ ] Appeller le tout 

  ```javascript
  animation.init() 
  ```

  

Exeple de fichier de base, pouvant être commun à tout projet canvas  pour un projet canvas à taille fix. 

``` javascript
//import { controller } from "./controller";


const animation = {
    canvasElt: undefined, 
    ctx: undefined, 
    //controller,

    init(){
        this.canvasElt = document.createElement("canvas")
        document.body.insertAdjacentElement("beforeend", this.canvasElt);
        this.canvasElt.height = 600;
        this.canvasElt.width = 800;
        this.ctx = 				this.canvasElt.getContext('2d'); 
      
      //Ajouter ici les nouvelles classes 
      
      //* initialiser mon controleur 

      
      this.animate();
    },

    draw(){
      // Ajouter ici les uptade
    },

    animate(){ 
        this.ctx.clearRect(0, 0, this.canvasElt.width, this.canvasElt.height);
        this.draw()
        window.requestAnimationFrame( ()=> {
            this.animate(); 
        });
    },
}

animation.init() 
```





### Création d'une classe

- [ ] Crée un fichier Nom_de_classe.js ı *Le fichier  **prend** une majuscule*

- [ ] Crée la nouvelle classe 

  ```javascript
  export default class Nom {
  	// La future classe 
  }
  ```

- [ ] Ajouter un construcor à la classe (qui comprendra toutes les variables). On y passe l'animation pour récupérer les truc présent dans app.js dont le canvas et le contexte qui seront nésessaire 

  ```javascript
  constructor(animation) {
          this.animation = animation;
          this.canvas = this.animation.canvasElt;
          this.ctx = this.animation.ctx
      }
  
  ```

  - [ ] Crée le draw() dans le quel on va mettre notre dessin (unique) 

    ```javascript
        draw(){         
            this.ctx.beginPath()
    				// La chose qu'on veux dessiner
            this.ctx.fill();
        }; 
    ```

  - [ ] Crée le update 

    ```javascript
        update(){
    			//Les modification qu'on veux voir à chaque frame d'animation ou si on utilise des contrôleurs 
        }
    ```

  exeple d'une classe de base :

  ```javascript
  export default class Bloc {
      constructor(animation) {
          this.animation = animation;
          this.canvas = this.animation.canvasElt;
          this.ctx = this.animation.ctx;
  
          this.color = "#ebd0bf";
          this.x = 25;
          this.y = 25;
          this.widht = 100;
          this.height = 50;
      }
  
  
      draw(){         
          this.ctx.beginPath()
          this.ctx.fillStyle = this.color
          this.ctx.fillRect(this.x, this.y, this.widht, this.height);
          this.ctx.fill();
      }; 
  
      update(){
  
  			this.draw(); 
      }
  }
  ```

  

  ##### SI l'object doit de trouvé sur la page en plusieurs exemplaires : 

  - [ ] Crée un tableau de la classe dans les variables de l'animation

    ```javascript
      nom: [],
      nomNb: x; 
    ```

  - [ ] push dans le tableau autant d'object qu'on en veux 

    ```javascript
    for(let i = 0; i < this.nomNb; i++){
                this.nom.push(new Nom(this));
            }
    ```

  - [ ] Ajouter l'update dans le draw si ça bouge  

    ```javascript
     this.shots.forEach( shot => {
              if(shot !== undefined){
                    shot.update();
                }
     })
    ```



Attention, si les éléments doivent être positionner d'une certaines façon sur le page et que les param ne sont pas "Math.random" il fallait le prévoir dans le class directement, avec des calcules parfois très perturbant. Exemple pour le cassBrique. Le **modulo** (reste d'une division) peut alors être utile.  

```javascript
        this.x = this.margin + Math.floor(this.i % 6) * (this.widht + this.margin);
        this.y = this.margin + Math.floor(this.i / 6) * (this.height + this.margin);
```



#### Si l'object doit de trouvé sur la page en plusieurs exemplaires : 

- [ ] ajouter le ship dans le init

  ```javascript
          this.nom = new Nom(this);
  ```

- [ ] Ajouter l'update dans le draw 

  ```javascript
          this.nom.update();
  ```



### Création du contrôleur 

- [ ] Crée un fichier controller.js ı *Le fichier ne prend pas de majuscule*

  ```javascript
  controller.js
  ```

- [ ] Crée une variable pour le controleur avec export

  ```javascript
  export const controller = {
  
  }
  ```

- [ ] ajouter un init avec animation en paramètre et un "isKeyDown" avec "key" (ou des autres mots selon le cas)

  ```javascript
      init(animation){
      },
  
      isKeyDown(key){
      },
  ```

- [ ] ajouter des tableaux avec les clef (pressed et allowed) et ajouter nos controleurs dans allowed

  ```javascript
      keyPressed : [],
      keyAllowed : ["q", "d"], // Les touches qu'on veux pourvoir utiliser 
  ```

  - [ ] **NOTE** : Dans ses vidéos, Villain met allowed dans un object pas dans un tableau 

    ```javascript
     keyAllowed : {
       'ArrowRight':1
       'ArrowLeft': -1 
     }
    ```

    

- [ ] ajouter un event listenr avec une arrow fonction pour quand on appuie sur une touche // quand on la relâche

  ```javascript
          window.addEventListener("keydown", e => {
  					
            
            
          }, false )
  
          window.addEventListener("keyup", e => {
            
            
          }, false )
  ```

- [ ] Dans l'eventListner down vérifier si la clef 'e' (e.key) est dans le tableau des clef utilisables et n'est pas déjà dans le tableau des clef pressée, on l'ajoute au tableau/ pour 

  ```javascript
              const key = e.key;
              if(this.allowedKeys.indexOf(key) != -1){
                  if(this.pressedKeys.indexOf(key) == -1){
                    e.preventDefault() // Ajout du prof pour éviter d'éventuelles problémes 
            e.stopPropagation() // Ajout du prof pour éviter d'éventuelles problémes 
                      this.pressedKeys.push(key);
                  }
  ```

- [ ] Dans l'eventListent up vérifier si la touche est bien dans le tableau et si c'est le cas la supprimer 

  ```java
            const key = e.key;
              if(this.pressedKeys.indexOf(key) != -1){  
  e.preventDefault() // Ajout du prof pour éviter d'éventuelles problémes 
            e.stopPropagation() // Ajout du prof pour éviter d'éventuelles problémes 
    this.pressedKeys.splice(this.pressedKeys.indexOf(key), 1);
              }
  ```

- [ ] Is Keydown sera utilisé plus tard pour savoir si la clef et ou non dans le tableau (pour affecter des acction quand la touche est utilisée )

  ```javascript
  isKeyDown(key){
          if(this.pressedKeys.indexOf(key) != -1){
              return true;
          }else{
              return false;
          }
      }
  ```

  

Exemple d'un fichier controleur fonctionelle 

```javascript
export const controller = {
    pressedKeys : [],
    allowedKeys : ["q", "d"],

    init(animation){
        window.addEventListener("keydown", e => {

            const key = e.key; // Recupére le nom de la touche appuiller
            if(this.allowedKeys.indexOf(key) != -1){ // index of renvoi -1 si l'élément n'est pas dans le tableau. Si la touche qu'on vient de presse est dans le tableau des touches autorisées/ 
                if(this.pressedKeys.indexOf(key) == -1){ // vétifie si la touche n'est pas déjà appuiller (pour que ça se fasse qu'une fois)
                    this.pressedKeys.push(key); // Alors on rajoute la touche dans touche appuillée 
                }
            }
        }, false )

        window.addEventListener("keyup", e => {

            const key = e.key; // voir plus haut
            if(this.pressedKeys.indexOf(key) != -1){ // On vérifie sur la touche est bien enfoncé, par sécuritée. 
                this.pressedKeys.splice(this.pressedKeys.indexOf(key), 1); // Splice à besoin d'un indice de départ (qu'on récup avec indexOf) et d'un nombre de truc à suprimer, ici un seul. 
            }

        }, false )
    },

    isKeyDown(key){
        if(this.pressedKeys.indexOf(key) != -1){
            return true; // si la touche est appuiller, on envoie true, on s'en servira plus tard =) 
        }else{
            return false;
        }
    },
}
```



#### Utilisation du contrôleur dans la classe (ou n'imorte ou dans mon annimation)

- [ ] lier le controller au fichier app.js

  ```javascript
  import { controller } from "./controller";
  ```

- [ ] Ajouter le controleur dans l'annimation 

  ```javascript
     controller, 
       OU 
  	 controller : controller,
  ```

- [ ] initialiser le controleur dans l'app.js (dans le init)

  ```javascript
          this.controller.init(this);
  ```

- [ ] Dans la classe de l'element qui va être impacter par le controleur 

  ```javascript
   if(this.animation.controller.isKeyDown("key")){
              // Ajouter la modificatyion à apporter quand la touche est utilisée.  
          }
  ```

  





## Element particuler d'un jeu

### Le déplacement : Les vecteurs

- [ ] Ajouter une classe Vertor avec un x et un y dans le constructor

  ```javascript
  export default class Vector {
      constructor(x, y){
          this.x = x; 
          this.y = y; 
      }
  }
  ```

- [ ] A la place de x et y 

  ```javascript
          this.location = new Vector(x, y);
  ```

- [ ] Pour crée un déplacement de l'objet 

  ```javascript
      add(vector) {
          this.x += vector.x;
          this.y += vector.y;
      }
  ```

- [ ] A la place de x et y 

  ```javascript
          this.location = new Vector(x, y);
  ```

- [ ] Pour crée un déplacement de l'objet 

  ```javascript
  const x = this.speed * Math.cos(this.angle)
          const y = this.speed * Math.sin(this.angle)
          const newCoo = new Vector(x, y)
          this.location.add(newCoo)
  ```

- [ ] A la place de x et y 

  ```javascript
   dist(vector) {
          const a = this.x - vector.x
          const b = this.y - vector.y
          const c = Math.sqrt((a*a) + (b*b))
          return c; 
      }
  ```

- [ ] Pour crée un déplacement de l'objet 

  ```javascript
          const dist = this.location.dist(this.ship.location);
  
  ```

  

### Le déplacement : Les angles 

- [ ] Ajouter un angle en degré a la classe.

  ```javascript
       this.angleDeg = 90; 
  ```

- [ ] Transformer l'angle en radien (utilisé par js)

  ```javascript
          this.angle = this.angleDeg * (Math.PI/180)
  ```

- [ ] S'assuer que l'élément à une vitesse 

  ```javascript
           this.speed = x; 
  ```

- [ ] Ajouter se petit bout de code dans le update() qui gére le déplacement avec l'angle. 

  ```javascript
          this.x += this.speed * Math.cos(this.angle)
          this.y += this.sp eed * Math.sin(this.angle)
  ```

- [ ] Pour modifier le déplacement gauche droit d'un object, modifier son angle (par exeple dans l'utilisation d'un contrôleur )

  ```javascript
  	this.angle = this.angle - 1
  
  ____________________________
  
    if(this.animation.controller.isKeyDown("q")){
              this.angle--; 
    }
  ```

  



### Détection de collision 

#### Pour les cercles (Pytagore)

##### Sans Boucle 

- [ ] Ajouter une méthode pour calculer les distances dans la classe Vector qui utilise le calcule de pytagore 

  ```javascript
      dist(vector) {
          const a = this.x - vector.x
          const b = this.y - vector.y
          const c = Math.sqrt((a*a) + (b*b))
          return c; 
      }
  ```

- [ ] Crée une méthode pour la colision dans une des deux classe consernée (Dans celle qui n'est pas "unique" si une est répètée, pour éviter les boucles et simplifier le truc)

  ```javascript
        boum(){
  
      }
  ```

- [ ] Importer l'autre objet dans le contstructor

  ```javascript
   		     this.className = this.animation.className; 
  ```

- [ ] Grace à la méthode de calcule de distance comparter les distance des deux objets

  ```javascript
       boum(){
          const dist = this.location.dist(this.className.location);
      }
  
  ```

- [ ] Ajouter une condition dans le cas ou il y a colision avec le résultat qu'on veux voir si c'est le cas 

  ```javascript
   	       if (this.raduis + this.ship.shipH/2 > dist){
              this.ship.shipMaxSpeed = 0;
  
          }
  ```

##### Avec Boucle

// A venir 



#### Pour des rectangles 

// A venir 







## Elements souvant utile 

### Les boucles 

##### Boucle for

```javascript
        for(let i = x; i < y; i++){
            
        }

let i = x // Au départ i est égale à x (généralement 0 ou 1). Quand on ajoute des choses dans un tableau x doit être égale à 0 car l'intex d'un tableau est commence à 0
i < y // y est jusqu'ou on veux aller. 
i++ // A ne pas changer, on incémente i a chaque fois
```

##### Boucle forEach

La boucle forEach est utile pour parcourire un tableau 

```javascript
this.tableauName.forEach(Name => (
             
        ))
```



### Math.random

Math random est une facon de définir un nombre aléatoire entre deux sommes.

```javascript
Math.random() * diférence + min;
```



### Modulo

Le modulo, en théorie permet de calculer le résultat d'une division mais en pratique il s'utilise pour aller jusqu'a un certain point. 

```javascript
% 
```

```javascript
this.i % 6 // Jusqu'a l'index 6 (ou 5 ?) __ i représent l'index d'un objet dans un tableau dans l'exeple.
```



### Math.floor

Math floor permet d'arrondire un nombre à l'unité inférieur

```javascript
Math.floor(n)
```

Note : `Math.ceil` permet de monter à l'unitée supérieur et `Math.round` a l'unitée le plus proche 



#### Math.sqrt 

Math sqrt permet de calculer la racine carrée. C'est nottement utile pour l'utilisation de pytagore dans le calcule de distance entre deux cercle.

```javascript
Math.sqrt()
```

Note : Il n'y a pas de "math" pour mettre un nombre au carré, pour se faire il faut faire `x * x`

# ENGINYERIA-INVERSA-DE-CODI-PROJECTE-JAVAS-SCRIPT-
# Informe Tècnic — Anàlisi del repositori *space-invaders* (ImKennyYip)

**Repo original analitzat:** https://github.com/ImKennyYip/space-invaders  
**Tecnologia:** JavaScript + HTML5 Canvas  
**Objectiu:** Documentar detalladament l’estructura i funcionament del joc Space Invaders per comprendre la seva lògica i aplicació a un projecte propi.

---

## 1. Principals variables que representen l’estat del joc

### Tauler (Canvas)
- `board`, `context` — referència al canvas i context 2D.
- `boardWidth`, `boardHeight` — dimensions.
- `tileSize`, `rows`, `columns` — definició de graella i mida base.

### Nau del jugador
- `shipWidth`, `shipHeight`
- `shipX`, `shipY`
- `shipVelocityX`
- `ship` — objecte amb posició, mides i imatge.
- `shipImg` — sprite.

### Aliens
- `alienArray` — llista d'enemics.
- `alienWidth`, `alienHeight`
- `alienX`, `alienY`
- `alienRows`, `alienColumns`
- `alienCount` — aliens vius.
- `alienVelocityX`
- `alienImg` — sprite enemic.

### Bales
- `bulletArray`
- `bulletVelocityY`

### Estat del joc
- `score`
- `gameOver`

---

## 2. Funcions que modifiquen l’estat del joc

### `window.onload`
La funció window.onload actua com a punt d’inicialització del joc. Quan la pàgina es carrega:
**-Inicialitza variables globals** que controlen el funcionament del joc (velocitats, dimensions, estats).
**-Carrega les imatges principals (nau, alien, explosió)**. Aquestes imatges es fan servir durant tot el joc, i per això s’han de tenir carregades abans de començar.
**-Crea els aliens inicials** cridant createAliens().
**-Configura el bucle principal del joc**, que s’executa mitjançant requestAnimationFrame(update).
**-Afegeix els listeners** d’interacció perquè el jugador pugui moure la nau i disparar.

Impacte en l’estat: Aquesta funció estableix l'estat inicial del joc i prepara tot el necessari perquè la lògica pugui començar.

### `createAliens()`
**Aquesta funció és responsable de generar la matriu d’aliens enemics.**
-Reinicia **alienArray**, que conté tots els aliens actius.
-Genera aliens de forma ordenada amb files i columnes.
-Assigna a cada alien propietats com:
       ·x, y (posició)
       ·width, height
       ·alive = true

Reinicia també **alienCount**, variable clau per saber quants aliens queden
Aquesta funció defineix l'estat inicial d’un nivell nou.

### `moveShip(e)`
**Gestió de moviments horitzontals de la nau del jugador.**
-Comprova la tecla premuda (ArrowLeft, ArrowRight).
-Actualitza ship.x segons la direcció.
-Limita el moviment perquè no surti fora del canvas.

Modifica contínuament la posició de la nau.
- Mou la nau (canvia `ship.x`).

### `shoot(e)`
Quan el jugador dispara:
-Detecta si s’ha premut la tecla Space.
-Crea un nou objecte bala amb propietats:
       ·x, y: apareix al centre de la nau
       ·width, height
       ·used = false: indica si la bala ja ha col·lisionat
-Afegeix la bala a bulletArray.
Afegeix nous projectils al joc.


### `update()`
Aquesta és la funció més important del joc. Actua com el bucle principal, executant-se contínuament amb requestAnimationFrame().

Dins la funció, es fan múltiples actualitzacions d’estat:

**· Moviment i actualització d'aliens**
-Actualitza la seva posició (x, y).
-Controla canvis de direcció quan toquen una vora.
-Baixa tota la matriu d’aliens quan cal.
-Marca aliens com a "morts" (alive = false) quan hi ha col·lisions.

**· Moviment de bales**
-Cada bala puja verticalment.
-Si surt del canvas, s’elimina.
-Si toca un alien, es marca com used = true

**· Detecció de col·lisions**
Comprova la sobreposició de rectangles entre:
-Bales i aliens
-Aliens i el jugador (final de la partida)

Quan hi ha col·lisió:
-Es modifica puntuació.
-Es redueix alienCount.
-Es reprodueix una animació d'explosió.

**· Control de nivells**
Quan alienCount arriba a 0:
-Es crea un nou conjunt d'aliens (createAliens()).
-S’incrementa la velocitat dels enemics.

**· Control de condicions de derrota**
Si un alien arriba massa avall:
-gameOver = true
-S’atura la lògica del joc i només es mostra el missatge.

**· Renderització (repintat)**
La funció també dibuixa constantment:
-Fons
-Nau del jugador
-Aliens vius
-Projectils
-Puntuació i estat de partida

---

##  3. Interacció amb el DOM (Canvas)

El joc empra la Canvas API:

- `drawImage()` — dibuixa sprites.
- `fillRect()` — dibuixa bales.
- `clearRect()` — neteja el canvas.
- `fillText()` — dibuixa puntuació.

Tot es dibuixa des de `update()` cada frame.

---

## 4. Control del flux del joc

### **`update()`**
És la funció que controla tot:
- Moviment
- Animacions
- Col·lisions
- Nivells
- Render
- Game Over

És cridada contínuament amb `requestAnimationFrame(update)`.

---

## 5. Events escoltats

| Event | Handler | Funció |
|-------|---------|---------|
| `keydown` | `moveShip()` | Mou la nau |
| `keyup` | `shoot()` | Dispara |

---

## 6. Temporització del joc

El joc utilitza:

### `requestAnimationFrame(update)`

No es fan servir `setInterval` ni `setTimeout`.

---

## 7. Control d’animacions

Les animacions funcionen així:
- Actualitzar posicions cada frame.
- Invertir direcció dels aliens quan arriben a un límit.
- Desplaçar els aliens cap avall a cada "gir".
- Redibuixar-ho tot des de `update()`.

Això és animació basada en fotogrames.

---

## 8. Detecció de col·lisions

S’utilitza AABB (Axis-Aligned Bounding Box):

```js
return a.x < b.x + b.width &&
       a.x + a.width > b.x &&
       a.y < b.y + b.height &&
       a.y + a.height > b.y;

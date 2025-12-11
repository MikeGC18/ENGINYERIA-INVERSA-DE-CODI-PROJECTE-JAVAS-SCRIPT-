# ENGINYERIA-INVERSA-DE-CODI-PROJECTE-JAVAS-SCRIPT-
# 📘 Informe Tècnic — Anàlisi del repositori *space-invaders* (ImKennyYip)

🔗 **Repo original analitzat:** https://github.com/ImKennyYip/space-invaders  
🎮 **Tecnologia:** JavaScript + HTML5 Canvas  
📝 **Objectiu:** Documentar detalladament l’estructura i funcionament del joc Space Invaders per comprendre la seva lògica i aplicació a un projecte propi.

---

## 🛠️ 1. Principals variables que representen l’estat del joc

### 🧩 Tauler (Canvas)
- `board`, `context` — referència al canvas i context 2D.
- `boardWidth`, `boardHeight` — dimensions.
- `tileSize`, `rows`, `columns` — definició de graella i mida base.

### 🚀 Nau del jugador
- `shipWidth`, `shipHeight`
- `shipX`, `shipY`
- `shipVelocityX`
- `ship` — objecte amb posició, mides i imatge.
- `shipImg` — sprite.

### 👾 Aliens
- `alienArray` — llista d'enemics.
- `alienWidth`, `alienHeight`
- `alienX`, `alienY`
- `alienRows`, `alienColumns`
- `alienCount` — aliens vius.
- `alienVelocityX`
- `alienImg` — sprite enemic.

### 🔫 Bales
- `bulletArray`
- `bulletVelocityY`

### 🎯 Estat del joc
- `score`
- `gameOver`

---

## 🔄 2. Funcions que modifiquen l’estat del joc

### `window.onload`
- Inicialització general.
- Carrega imatges.
- Genera aliens.
- Configura `update()` i events.

### `createAliens()`
- Genera aliens en forma de graella.
- Reinicia `alienArray` i `alienCount`.

### `moveShip(e)`
- Mou la nau (canvia `ship.x`).

### `shoot(e)`
- Afegeix bales a `bulletArray`.

### `update()`
Funció central del joc:
- Mou aliens i bales.
- Detecta col·lisions.
- Actualitza puntuació.
- Gestiona nivells.
- Controla el final del joc.
- Renderitza l'estat actual.

---

## 🎨 3. Interacció amb el DOM (Canvas)

El joc empra la Canvas API:

- `drawImage()` — dibuixa sprites.
- `fillRect()` — dibuixa bales.
- `clearRect()` — neteja el canvas.
- `fillText()` — dibuixa puntuació.

Tot es dibuixa des de `update()` cada frame.

---

## 🎮 4. Control del flux del joc

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

## ⌨️ 5. Events escoltats

| Event | Handler | Funció |
|-------|---------|---------|
| `keydown` | `moveShip()` | Mou la nau |
| `keyup` | `shoot()` | Dispara |

---

## ⏱️ 6. Temporització del joc

El joc utilitza:

### ✔️ `requestAnimationFrame(update)`

No es fan servir `setInterval` ni `setTimeout`.

---

## 🌀 7. Control d’animacions

Les animacions funcionen així:
- Actualitzar posicions cada frame.
- Invertir direcció dels aliens quan arriben a un límit.
- Desplaçar els aliens cap avall a cada "gir".
- Redibuixar-ho tot des de `update()`.

Això és animació basada en fotogrames.

---

## 💥 8. Detecció de col·lisions

S’utilitza AABB (Axis-Aligned Bounding Box):

```js
return a.x < b.x + b.width &&
       a.x + a.width > b.x &&
       a.y < b.y + b.height &&
       a.y + a.height > b.y;

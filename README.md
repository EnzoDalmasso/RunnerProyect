# RunnerProyect

Prototipo de un runner por carriles hecho en Unreal Engine 5.3, todo con Blueprints. El personaje avanza solo y el jugador lo mueve de carril para esquivar obstáculos y juntar monedas hasta cruzar la línea de llegada. Si choca, el nivel arranca de nuevo.

Lo armé entre fines de noviembre y principios de diciembre de 2024, partiendo del template Third Person de Unreal.

## Cómo abrirlo

Hace falta Unreal Engine 5.3. Clonar el repo, abrir `ProyectoRunner.uproject` y darle Play; el mapa por defecto ya es `RunnerGameMap`.

Controles: A y D para cambiar de carril, Espacio para saltar.

## Qué hay adentro

Todo lo que hice yo está en `Content/RunnerGame/`. El resto (personaje, animaciones, input base) viene del template.

**Cambio de carril.** Las posiciones X de cada carril están en un array (`LanePositions`) dentro de `BP_RunnerGameCharacter`, y el personaje guarda en qué carril está (`CurrentLane`). Al apretar una tecla calculo el carril destino con un `Clamp` para no pasarme de los bordes, y una Timeline (`SmoothTransition`) hace un `Lerp` entre la X actual y la del carril nuevo. Así el movimiento es suave y no un teletransporte.

**Monedas.** `BP_Item` es la moneda: gira con un `RotatingMovement` y cuando el personaje la toca llama a `AddCoin`, que suma a `TotalCoins`. El widget `UW_Coins` lee ese valor con un binding y lo muestra en pantalla. Lo agrega al viewport `BP_RunnerGameMode` en el `BeginPlay`.

**Obstáculos y meta.** `BP_Obstacle` detecta el golpe con `OnComponentHit` y dispara el evento `Death` del personaje, que vuelve a abrir el nivel actual. `BP_FinishLine` muestra el widget de victoria (`UW_Win`), espera unos segundos con un `Delay` y también reinicia el nivel.

```
Content/RunnerGame/
├── BluePrints/   personaje, game mode, moneda, obstáculo, meta, widget de victoria
├── Maps/         RunnerGameMap
├── Materials/
└── Widgets/      contador de monedas
```

## Pendiente

Me quedó sin hacer el menú de pausa y una pantalla de inicio. Lo siguiente que le agregaría es que los tramos de pista se generen solos en lugar de estar armados a mano en el mapa, para que la carrera no tenga un final fijo.

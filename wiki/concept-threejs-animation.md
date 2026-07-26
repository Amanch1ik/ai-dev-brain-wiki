# Three.js: система анимации

«Полноценный микшерный пульт»: анимирует почти любое свойство — position, scale, rotation, цвет/прозрачность материала, кости скелета, морф-таргеты — и умеет **смешивать** анимации (переход из walk в run).

## Содержание

Система работает на **ключевых кадрах**: ставишь значения в точках времени, промежутки досчитываются автоматически (**tweening**). Сложность определяет плотность кадров; больше 60 к/с смысла нет на 60 Гц экране.

### Создание: keyframes → KeyframeTrack → AnimationClip
**Keyframe** = время + свойство + значение («на 0 сек `.position` = (0,0,0)»). Кадры не привязаны к конкретному объекту, но задают **тип данных**:

| Тип | Примеры |
|---|---|
| Number | `material.opacity`, `camera.zoom` |
| Vector | `Object3D.position`, `.scale` |
| **Quaternion** | `Object3D.quaternion` |
| Boolean | `material.wireframe`, `light.castShadow` (прыжком, без промежутков) |
| String | редко |

⚠ **Euler-углов в списке нет.** Чтобы анимировать вращение, используй `Object3D.quaternion`, а не `.rotation`.

`KeyframeTrack` собирает кадры для одного свойства, `AnimationClip` — набор треков (одна законченная анимация, например «полёт»).

### Воспроизведение: AnimationMixer → AnimationAction
```js
const mixer = new THREE.AnimationMixer(model);
const action = mixer.clipAction(gltf.animations[0]);
action.play();

// в цикле рендера — обязательно:
mixer.update(delta);   // см. concept-threejs-render-loop
```
`AnimationAction` даёт контроль: `play/stop/paused`, `timeScale` (скорость), `loop`, `weight` (вес для смешивания), `crossFadeTo(other, duration)` — плавный переход между клипами.

### Важная практика
Систему three.js **редко используют для ручной анимации в коде** — она заточена под клипы из внешних программ (Blender, приходят внутри [[concept-threejs-gltf]]). Для анимации из кода берут [[lib-gsap]] (или Tween.js): удобнее таймлайны и easing.

**Скелетная анимация** (SkinnedMesh + кости) и **морф-таргеты** (шейпкеи) грузятся из glTF и управляются тем же микшером.

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-threejs-gltf]] — клипы приходят внутри моделей
- [[concept-threejs-render-loop]] — `mixer.update(delta)` каждый кадр
- [[lib-gsap]] — предпочтительно для анимации из кода
- [[concept-threejs-scene-graph]] — что именно анимируем

## Источник

- raw/d3-animation-system.md — Discover three.js, «The three.js Animation System»

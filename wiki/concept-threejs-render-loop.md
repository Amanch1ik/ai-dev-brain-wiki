# Three.js: цикл рендера и анимация

Один `renderer.render(scene, camera)` даёт статичный кадр. Для движения нужен цикл, синхронизированный с частотой экрана.

## Содержание

```js
const clock = new THREE.Clock();

renderer.setAnimationLoop(() => {      // предпочтительнее requestAnimationFrame
  const delta = clock.getDelta();      // секунд с прошлого кадра
  mesh.rotation.y += 1.2 * delta;      // скорость в единицах/сек — не зависит от FPS
  renderer.render(scene, camera);
});
```

- **`setAnimationLoop`** — встроенная замена `requestAnimationFrame`; обязательна для WebXR.
- **Всегда умножай на `delta`.** Иначе на 144 Гц анимация будет вдвое быстрее, чем на 60 Гц.
- `clock.getElapsedTime()` — общее время, удобно для синусоид/шейдерных uniform'ов.

### Ресайз
```js
window.addEventListener('resize', () => {
  camera.aspect = w / h;
  camera.updateProjectionMatrix();   // ⚠ без этого картинка растянется
  renderer.setSize(w, h);
});
```

### Гигиена цикла (прямо влияет на FPS)
- **Не аллоцируй в цикле** — не создавай `new Vector3()` каждый кадр; заводи переменные снаружи и переиспользуй.
- Не делай тяжёлых DOM-операций и `console.log` в кадре.
- Один общий RAF на всё приложение: свяжи скролл ([[lib-lenis]]), GSAP-таймлайны ([[lib-gsap]]) и рендер в одном цикле, а не в трёх параллельных.
- Останавливай цикл, когда канвас вне экрана (`IntersectionObserver`) — экономит батарею.
- Для reduced-motion — спокойный режим ([[concept-reduced-motion]]).

В React всё это уже обёрнуто: `useFrame((state, delta) => …)` в [[lib-react-three-fiber]].

## Связано с

- [[lib-three-js]] — хаб библиотеки
- [[concept-threejs-scene-graph]] — что именно рендерим
- [[lib-react-three-fiber]] — `useFrame` как тот же цикл
- [[concept-webgl-performance]] — аллокации и FPS
- [[lib-lenis]] · [[lib-gsap]] — синхронизация в одном RAF
- [[concept-reduced-motion]] — спокойный режим

- [[concept-threejs-animation]] — `mixer.update(delta)` в цикле
- [[concept-threejs-controls]] — `controls.update()` в цикле
- [[concept-threejs-postprocessing]] — рендерим composer
- [[concept-threejs-raycasting]] — когда вызывать raycast

- [[method-immersive-web-interaction-patterns]] — pointer-lerp через MathUtils.lerp в useFrame
- [[method-webgl-performance-degradation]] — on-demand loop, аллокации в useFrame

## Источник

- raw/d3-animation-loop.md — Discover three.js, «The Animation Loop»

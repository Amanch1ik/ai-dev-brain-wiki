# WebGL: адаптивная деградация и бюджет кадра

Рантайм-стратегии держать 60fps в R3F: on-demand рендер, динамика качества, деградация на мобиле, решение «WebGL или CSS». Практический слой к [[concept-webgl-performance]].

## Содержание

### On-demand рендер
Если сцена статична между интеракциями — не гоняй кадры впустую:
- `<Canvas frameloop="demand">` — рендер только по запросу.
- `invalidate()` — запросить кадр (после изменения стейта/твина). GSAP-твины дергают `invalidate` через `onUpdate`.
- Экономит батарею и GPU на страницах, где 3D — не постоянная анимация, а реакция на действие.

### Динамика качества
- `<PerformanceMonitor onDecline onIncline>` (drei) — следит за FPS и даёт крутить качество на лету: снижать `dpr`, отключать пост-эффекты, резать частицы при `onDecline`, возвращать при `onIncline`.
- `regress()` — временно снизить качество на время активного взаимодействия (drag/scroll), потом вернуть.

### Базовая гигиена ресурсов
- **Шарь geometry/material** между мешами — не плоди дубликаты.
- `useLoader` кеширует по URL — один и тот же ассет грузится once.
- **LOD** через `<Detailed>` (drei): дальним мешам — упрощённая геометрия.
- Не аллоцируй в `useFrame` (переиспользуй векторы) — см. [[concept-threejs-render-loop]].

### Когда WebGL, а когда CSS/2D-transform
Не каждый эффект стоит WebGL:
- **WebGL** — одна hero/wow-сцена на страницу (product viewer, particle-фон, shader-hero).
- **CSS / 2D transform** — hover-карточки, списки, reveal, параллакс-слои: дешевле, стабильнее на мобиле, не грузит GPU-контекст. Дешёвый CSS часто **неотличим** от WebGL для мелкой интеракции.

Правило: WebGL там, где без 3D/шейдера эффект в принципе не собрать. Остальное — CSS/[[lib-framer-motion]].

### Мобильная деградация
- `dpr={[1, 1.5]}` (не `2`) — на Retina полный `devicePixelRatio` учетверяет пиксели.
- Резать post-processing и частицы по `navigator.hardwareConcurrency` (мало ядер → упрощённая сцена).
- На слабых устройствах — **подмена статикой** (poster-изображение вместо сцены).
- **reduced-motion в R3F — руками** через `window.matchMedia('(prefers-reduced-motion: reduce)')`: R3F не знает про [[concept-reduced-motion]] сам, спокойный режим включаешь кодом.

### Грабли
- ⚠ drei `<Instances>` может **просаживать FPS** против vanilla `InstancedMesh` (pmndrs/react-three-fiber#3306) — на больших количествах тестируй и при регрессе падай на чистый `InstancedMesh`.

## Связано с

- [[concept-webgl-performance]] — базовые рычаги (текстуры, draw calls, dpr)
- [[lib-react-three-fiber]] — `frameloop`, `invalidate`, `Canvas`
- [[lib-drei]] — `PerformanceMonitor`, `Detailed`, `Instances`
- [[concept-reduced-motion]] — спокойный режим 3D через matchMedia
- [[method-immersive-web-interaction-patterns]] — бюджет кадра immersive-сцены
- [[method-webgl-mobile-rn-porting]] — деградация при переносе на нативный мобайл
- [[method-premium-creative-web]] — перформанс как non-negotiable

## Источник

- r3f.docs.pmnd.rs/advanced/scaling-performance — on-demand, PerformanceMonitor
- pmndrs.github.io/drei — Detailed, Instances, PerformanceMonitor
- github.com/pmndrs/react-three-fiber/issues/3306 — Instances FPS-регресс ⚠ проверить актуальность

# Перформанс WebGL / 3D

Лагающий 3D читается как «дёшево», а не «дорого» — поэтому производительность в премиум-вебе не опция, а требование. Цель: стабильные 60fps. Часть non-negotiables [[method-premium-creative-web]].

## Содержание

Ключевые рычаги:
- **Текстуры** — самый частый источник тормозов: сжимай в **KTX2/Basis** (GPU-compressed), ограничивай размеры, генерируй mipmaps. Тяжёлые PNG/JPEG → долгая загрузка и память.
- **Pixel ratio** — cap `dpr` (в [[lib-react-three-fiber]] — `dpr={[1, 2]}`): на Retina рендер в полный `devicePixelRatio` учетверяет число пикселей.
- **Draw calls** — instancing (`InstancedMesh`) для тысяч одинаковых объектов; объединяй геометрию; LOD для дальних мешей.
- **Шейдеры** — тяжёлые фрагментные шейдеры ([[concept-glsl-shaders]]) считаются на каждый пиксель; профилируй, упрощай для мобильных.
- **Загрузка** — lazy-load 3D, Suspense + лоадеры (drei), прелоад критичного, декодинг вне主потока.
- **Loop-гигиена** — не аллоцировать в render-loop (`useFrame`), переиспользовать векторы/матрицы; синхронизировать со скроллом через один RAF ([[lib-lenis]]).

**Деградация:** на мобильных/слабых GPU — упрощённая сцена (меньше частиц, проще шейдеры, ниже разрешение), а не сломанная десктопная. Плюс `prefers-reduced-motion` — спокойный фолбэк.

## Связано с

- [[lib-three-js]] · [[lib-react-three-fiber]] — где применяются эти оптимизации
- [[lib-drei]] — Loader/Preload/LOD-хелперы
- [[concept-reduced-motion]] — спокойная версия 3D для reduced-motion
- [[concept-glsl-shaders]] — стоимость фрагментных шейдеров
- [[concept-scroll-driven-animation]] — держать 60fps при scroll-сценах
- [[method-premium-creative-web]] — перформанс как требование «дорогого» вида

- [[concept-threejs-geometry]] — инстансинг, dispose, LOD
- [[concept-threejs-gltf]] — Draco/KTX2, ленивая загрузка моделей
- [[concept-threejs-lights]] — тени как источник просадок
- [[concept-threejs-render-loop]] — аллокации в кадре и FPS
- [[concept-threejs-scene-graph]] — cap pixelRatio
- [[concept-threejs-textures]] — KTX2, размеры текстур

- [[concept-threejs-postprocessing]] — постобработка как главный потребитель GPU

- [[method-webgl-performance-degradation]] — рантайм-деградация, on-demand, mobile, WebGL vs CSS
- [[method-immersive-web-interaction-patterns]] — бюджет кадра immersive-сцены

## Источник

- скилл `~/.claude/skills/premium-creative-frontend/SKILL.md`
- raw/r3f-readme.md — R3F (render-loop, drei)

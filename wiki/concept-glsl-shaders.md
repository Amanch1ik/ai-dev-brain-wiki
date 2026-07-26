# GLSL-шейдеры

Программы на GLSL, исполняющиеся на GPU для **каждого пикселя одновременно**. Источник «дорогого» визуала (градиенты, noise, fluid, дисторшн) в [[method-premium-creative-web]].

## Содержание

По The Book of Shaders: шейдер — набор инструкций, но исполняемых **разом для каждого пикселя** экрана. Программа работает как функция, которая получает позицию и возвращает цвет; после компиляции выполняется чрезвычайно быстро (параллельно на GPU). Поэтому код должен вести себя по-разному в зависимости от позиции пикселя.

Два типа:
- **Vertex shader** — обрабатывает вершины геометрии (положение, деформация меша).
- **Fragment shader** — вычисляет цвет каждого фрагмента/пикселя (градиенты, паттерны, освещение, постэффекты).

В [[lib-three-js]] подключаются через `ShaderMaterial` (в R3F — drei `shaderMaterial`); данные из JS передаются как **uniforms** (время, разрешение, позиция мыши), интерполируемые значения — как **varyings**. Типичные приёмы «премиум»-вида: noise/FBM, displacement по шуму, fresnel/iridescence, дизеринг, fluid.

⚠ Требует понимания координат (нормализация по разрешению), GPU-профилирования — тяжёлые фрагментные шейдеры бьют по [[concept-webgl-performance]].

## Связано с

- [[lib-three-js]] — подключение шейдеров через ShaderMaterial
- [[concept-webgl-performance]] — стоимость фрагментных шейдеров
- [[lib-drei]] — `shaderMaterial` как удобный вход в шейдеры
- [[method-premium-creative-web]] — где шейдеры дают «дорогой» вид

- [[concept-threejs-geometry]] — кастомные атрибуты в вершинный шейдер
- [[concept-threejs-materials]] — ShaderMaterial как свой материал

- [[concept-threejs-postprocessing]] — свои эффекты как фрагментные шейдеры

- [[method-immersive-web-interaction-patterns]] — bulge/displacement/distortion в immersive
- [[method-webgl-mobile-rn-porting]] — SkSL как нативный аналог GLSL

## Источник

- raw/book-of-shaders-intro.md — The Book of Shaders (Patricio Gonzalez Vivo)

# Product Scope

## В системе

- Autodesk Revit 2025 add-in;
- scope: Active View / Entire Document / Current Selection;
- навигация Category → Family → Type;
- фильтрация по параметрам и синтетическим свойствам;
- логические условия AND / OR;
- selection actions: Replace / Add / Exclude;
- temporary visibility actions: Hide / Isolate / Inverse / Reset;
- создание совместимого штатного Revit `ParameterFilterElement`;
- reusable presets и templates;
- runtime lifecycle, caching и responsiveness mechanisms, необходимые для корректной работы внутри Revit.

## Вне продуктовой границы

- владение исходными BIM-данными модели;
- произвольное редактирование параметров и геометрии элементов;
- отдельная задача по корректировке DWG, присутствовавшая среди переданных материалов, но не относящаяся к ContextFilter.

Revit остаётся источником истины для документа, элементов, видов, selection и native API state.

# Atomic write

`JsonPresetStore` использует `AtomicFileWriter` для записи persisted preset document.

## Проблема

Прямое перезаписывание единственного файла создаёт риск оставить durable state частично записанным при сбое процесса, I/O error или неожиданном завершении.

## Контракт Infrastructure

```text
new document
→ serialize
→ write replacement safely
→ publish completed file
```

Пользователь не должен получить состояние, которое выглядит как валидный `presets.json`, но содержит только частичный результат записи.

## Что здесь канонично

- atomic write является persistence mechanic;
- Domain не знает о temporary files, rename/replace или filesystem;
- Application не должен зависеть от конкретного file-write protocol.

## Verification

В implementation evidence присутствуют отдельные `AtomicFileWriterTests`, то есть crash-safe file writing рассматривается как самостоятельная проверяемая Infrastructure responsibility.

## Граница evidence

Предоставленный анализ подтверждает использование `AtomicFileWriter`, но не раскрывает точную последовательность filesystem операций. Поэтому документ фиксирует гарантию и ответственность, а не придумывает внутренний алгоритм.

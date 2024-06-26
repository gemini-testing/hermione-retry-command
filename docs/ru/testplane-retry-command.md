# @testplane/retry-command

## Обзор

Используйте плагин `@testplane/retry-command`, чтобы ретраить отдельные команды на низком уровне.

## Установка

```bash
npm install -D @testplane/retry-command
```

## Настройка

Необходимо подключить плагин в разделе `plugins` конфига `testplane`:

```javascript
module.exports = {
    plugins: {
        '@testplane/retry-command': {
            enabled: true,
            rules: [
                {
                    condition: 'blank-screenshot',
                    browsers: ['MicrosoftEdge'],
                    retryCount: 5,
                    retryInterval: 120
                },
                {
                    condition: 'assert-view-failed',
                    browsers: ['Chrome'],
                    retryCount: 1,
                    retryOnlyFirst: true
                }
            ]
        },

        // другие плагины testplane...
    },

    // другие настройки testplane...
}
```

### Расшифровка параметров конфигурации

| **Параметр** | **Тип** | **По&nbsp;умолчанию** | **Описание** |
| :--- | :---: | :---: | :--- |
| enabled | Boolean | true | Включить / отключить плагин. |
| rules | Array | _N/A_ | Набор правил, в соответствии с которыми команды будут ретраиться. Обязательный параметр. |

### rules

Параметр `rules` представляет собой массив объектов. Каждый объект описывает отдельное правило для ретрая команд.

Из каких параметров состоит это правило:

| **Параметр** | **Тип** | **По&nbsp;умолчанию** | **Описание** |
| :--- | :---: | :---: | :--- |
| condition | String | _N/A_ | Условие для ретрая: _blank-screenshot_ или _assert-view-failed_. Подробнее см. ниже. |
| browsers | String/RegExp/Array | /.*/ | Список браузеров, в которых нужно применять ретраи. |
| retryCount | Number | 2 | Количество ретраев. |
| retryInterval | Number | 100 | Задержка перед каждым ретраем, в мс. |
| retryOnlyFirst | Boolean | false | Ретраить только первую команду в тесте. Опция актуальна только для условия _assert-view-failed_. |

### condition

Условие, при котором нужно ретраить. Всего доступны 2 значения:
* `blank-screenshot` &mdash; ретраить низкоуровневую команду снятия скриншота [takeScreenshot][take-screenshot], если в результате команды вернулся пустой скриншот;
* `assert-view-failed` &mdash; ретраить высокоуровневую команду testplane для снятия скриншота `assertView`, если она упала.

### browsers

Список браузеров, в которых нужно применять низкоуровневые ретраи для команд. Можно задавать как строку (если это один браузер), регулярное выражение или массив строк или регулярных выражений. По умолчанию, параметр `browsers` имеет значение `/.*/`, под которое попадают все браузеры.

### retryCount

Параметр определяет сколько раз нужно ретраить команду, если соблюдено условие, заданное в параметре `condition`.

### retryInterval
  
Параметр задает время в миллисекундах, которое надо подождать, прежде чем попытаться снова повторить команду.

_Будьте осторожны, устанавливая значение для данного параметра, так как слишком большое значение может драматически снизить скорость ваших тестов._

### retryOnlyFirst

Разрешает ретраить только первую команду `assertView` в тесте, если установлено значение `true`. Для других условий этот параметр не используется.

[@testplane/retry-command]: https://github.com/gemini-testing/testplane-retry-command
[take-screenshot]: https://webdriver.io/docs/api/webdriver/#takescreenshot

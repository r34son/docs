# Диалоги в {{ speechsense-name }}

_Диалог_ — это объект {{ speechsense-name }}, включающий в себя запись разговора сотрудника контактного центра (оператора) с клиентом и различные метаданные о разговоре: имена оператора и клиента, дату звонка, язык разговора и другие. Разговоры записывают в контактных центрах с помощью АТС, а затем [загружают](../operations/data/upload-data.md) в {{ speechsense-name }}.

В каждом разговоре {{ speechsense-name }} автоматически разделяет речь оператора и клиента и применяет к их фразам различные теги. Это помогает определить, например, поздоровался ли оператор, был ли доброжелательным клиент.

Диалоги бывают двух направлений:

* Исходящие — инициированные оператором.
* Входящие — инициированные клиентом.

С помощью {{ speechsense-name }} вы можете:

* просмотреть характеристики диалога;
* прослушать аудиозапись разговора;
* получить информацию о качестве работы оператора;
* просмотреть текстовую расшифровку аудиозаписи;
* просмотреть разметку текста определенными тегами.

Работать с диалогами можно несколькими способами:

* [Найти и просмотреть диалог](../operations/data/manage-dialogs.md) в списке диалогов.
* [Сформировать отчет](../operations/data/manage-reports.md) и [перейти](../operations/data/manage-reports.md#go-to-a-dialog) из него к диалогам. В этом случае к списку диалогов применяются критерии, заданные в отчете.

## Фильтрация в диалогах {#filters}

Фильтры определяют условия, по которым выполняется [поиск диалогов](../operations/data/manage-dialogs.md#filters-dialogs).

Фильтры делятся на четыре типа:

* **Оператор** — критерии качества работы оператора. Например, скорость речи оператора, прерывал ли он клиента.
* **Клиент** — особенности поведения клиента во время разговора. Например, скорость речи клиента, прерывал ли он оператора.
* **Общие метаданные** — данные о записанном разговоре.
* **Теги клиента** и **Теги оператора** — классификаторы, которые применяются к результатам распознавания разговора. [Подробнее о тегах](#tags).

Для каждого фильтра указывается одно или несколько значений — условий фильтрации. Значения могут быть трех типов:

* Дата — диапазон дат выбирается из календаря.
* Текстовый — вводится строка. Поиск выполняется по полному совпадению.
* Числовой — указывается диапазон чисел. Можно указать обе границы диапазона либо только одну из них. Чтобы найти конкретное значение, укажите его и в верхней, и в нижней границе. Значения границ включаются в диапазон фильтрации.

Можно использовать одновременно несколько фильтров. В этом случае будут найдены диалоги, которые удовлетворяют всем заданным условиям.

## Детальная информация о диалоге {#detail}

Для каждого диалога [доступна](../operations/data/manage-dialogs.md#view-dialog) информация:

* Общая информация о диалоге. Например, инициатор и длительность разговора.
* Сведения об операторе и клиенте.
* Аудиозапись разговора.
* Суммаризация (резюме), автоматически сгенерированная [{{ yagpt-full-name }}](../../yandexgpt/) на основе полного семантического анализа диалога. Представляет собой ответы {{ yagpt-full-name }} на вопросы в формате **Да**/**Нет**.
* Текстовая расшифровка аудиозаписи, автоматически выполненная с помощью [{{ speechkit-full-name }}](../../speechkit/).

    В расшифровке доступен [поиск по фрагменту текста](../operations/data/manage-dialogs.md#find-dialogs) в канале клиента или оператора. Ищется точное совпадение. Найденные фрагменты подсвечиваются желтым цветом.

    Текст расшифровки автоматически размечается тегами оператора и клиента.

## Теги в диалоге {#tags}

_Теги_ проставляются на диалоги с помощью классификаторов: {{ speechsense-name }} определяет, что в разговоре встретились определенные ключевые слова, фразы или интонации, классифицирует разговор и добавляет к нему тег. В {{ speechsense-name }} есть предустановленный набор тегов. Например, теги показывают, что оператор поблагодарил клиента за ожидание, или что клиент обращается в службу поддержки повторно.

В текстовой расшифровке диалога можно увидеть количество и перечень присвоенных тегов для каналов оператора и клиента. Если нажать на название тега, его цветом подсветятся те слова, к которым этот тег был отнесен.

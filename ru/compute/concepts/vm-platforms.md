# Платформы

{{ compute-full-name }} предоставляет различные виды физических процессоров. Выбор платформы гарантирует тип физического процессора в дата-центре и определяет набор допустимых конфигураций vCPU и RAM. Также, к виртуальной машине можно добавить графический ускоритель ([GPU](gpus.md)). Платформу необходимо выбирать при создании каждой виртуальной машины.

{% note warning %}

В зоне `{{ region-id }}-d` не поддерживаются ВМ на платформе Intel Broadwell. Чтобы [перенести](../operations/vm-control/vm-change-zone.md) ВМ с такой платформы в зону `{{ region-id }}-d`, вы можете:

* Сделать снимок диска и создать из него новую ВМ в зоне `{{ region-id }}-d` на другой платформе.
* Остановить ВМ, изменить платформу и переместить ВМ командой `relocate`.

{% endnote %}

## Стандартные платформы {#standard-platforms}


Платформа | Процессор | Макс. кол-во ядер (vCPU)</br> на виртуальной машине | Базовая тактовая</br> частота процессора, ГГц
--- | --- | --- | ---
Intel Broadwell</br>(`standard-v1`) | Intel® Xeon® Processor E5-2660 v4 | 32 | 2.00
Intel Cascade Lake</br>(`standard-v2`) | Intel® Xeon® Gold 6230 | 80 | 2.10
Intel Ice Lake</br>(`standard-v3`) | Intel® Xeon® Gold 6338 | 96 | 2.00





## Высокопроизводительные платформы {#compute-optimized-platforms}

Платформа | Процессор | Макс. кол-во ядер (vCPU)</br> на виртуальной машине | Базовая тактовая</br> частота процессора, ГГц
--- | --- | --- | ---
{{ highfreq-ice-lake }}</br>(`highfreq-v3`) | Intel® Xeon® Processor 6354 | 56 | 3.00


## Платформы с GPU {#gpu-platforms}


Платформа | Графический</br> ускоритель | Процессор | Характеристики
--- | --- | --- | --- 
Intel Broadwell with</br>NVIDIA® Tesla® V100</br>(`gpu-standard-v1`) | [NVIDIA® Tesla® V100](https://www.nvidia.com/ru-ru/data-center/tesla-v100/) | Intel® Xeon®</br>Processor E5-2660 v4 | **Макс. кол-во GPU на 1 ВМ**: 4 </br> **Кол-во vCPU на 1 GPU**: 8 </br> **Объем RAM на 1 GPU**: 96 ГБ
Intel Cascade Lake</br>with NVIDIA® Tesla® V100</br>(`gpu-standard-v2`) | [NVIDIA® Tesla® V100](https://www.nvidia.com/ru-ru/data-center/tesla-v100/) | Intel® Xeon® Gold 6230 | **Макс. кол-во GPU на 1 ВМ**: 8 </br> **Кол-во vCPU на 1 GPU**: 8 </br> **Объем RAM на 1 GPU**: 48 ГБ
AMD EPYC™</br>with NVIDIA® Ampere® A100</br>(`gpu-standard-v3`) | [NVIDIA® Ampere® A100](https://www.nvidia.com/ru-ru/data-center/a100/) | [AMD EPYC™ 7702](https://www.amd.com/ru/products/cpu/amd-epyc-7702) | **Макс. кол-во GPU на 1 ВМ**: 8 </br> **Кол-во vCPU на 1 GPU**: 28 </br> **Объем RAM на 1 GPU**: 119 ГБ
Intel Ice Lake with</br>NVIDIA® Tesla® T4</br>(`standard-v3-t4`) | [NVIDIA® Tesla® T4](https://www.nvidia.com/ru-ru/data-center/tesla-t4//) | Intel® Xeon® Gold 6338 | **Макс. кол-во GPU на 1 ВМ**: 1 </br> **Кол-во vCPU на 1 GPU**: 4, 8, 16 или 32 </br> **Объем RAM на 1 GPU**: 16, 32, 64 или 128 ГБ



## Смотрите также {#see-also}

* [Допустимые конфигурации vCPU и RAM](performance-levels.md).
* [Допустимые конфигурации GPU, vCPU и RAM](gpus.md#config).
* [Тарифы на вычислительные ресурсы для разных платформ](../pricing.md#prices).

# Интеграция экономической системы в проект Unity и обучение ML-Agent.
Отчет по лабораторной работе #4 выполнил(а):
- Загирова Ольга Сергеевна
- РИ210947
Отметка о выполнении заданий (заполняется студентом):

| Задание | Выполнение | Баллы |
| ------ | ------ | ------ |
| Задание 1 | * | 60 |
| Задание 2 | * | 20 |
| Задание 3 | * | 20 |

знак "*" - задание выполнено; знак "#" - задание не выполнено;

Работу проверили:
- к.т.н., доцент Денисов Д.В.
- к.э.н., доцент Панов М.А.
- ст. преп., Фадеев В.О.

[![N|Solid](https://cldup.com/dTxpPi9lDf.thumb.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Структура отчета

- Данные о работе: название работы, фио, группа, выполненные задания.
- Цель работы.
- Задание 1.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 2.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Задание 3.
- Код реализации выполнения задания. Визуализация результатов выполнения (если применимо).
- Выводы.
- ✨Magic ✨

## Цель работы
Научиться интегрировать экономическую систему в Unity и обучить ей пользоваться Ml Agent.

## Задание 1
### Изменить параметры файла .yaml-агента и определить какие параметры и
как влияют на обучение модели.

Ход работы:

- Открыть проект в Unity, который представлен в методических указаниях, и ознакомиться с ним.

- Запустите Anaconda Prompt (от имени администратора). Создайть виртуальное пространство.

![image](https://user-images.githubusercontent.com/92687732/205116832-7ff95f79-04df-47c2-9df9-b6d6769640c8.png)

![image](https://user-images.githubusercontent.com/92687732/198297233-3224905f-f116-4374-8bb1-192f32e90cf9.png)

![image](https://user-images.githubusercontent.com/92687732/198297280-7f1e59b8-8e19-458d-af96-c9e24914827d.png)

![image](https://user-images.githubusercontent.com/92687732/198297331-3b0477a6-2391-444f-a4d5-afe1b5b513a3.png)

![image](https://user-images.githubusercontent.com/92687732/198297406-0e77f139-4bae-439d-83b3-ada2f69116fa.png)

![image](https://user-images.githubusercontent.com/92687732/198297462-3f73a02f-7ca7-4d30-b0fd-a6ae546527cb.png)

- Перейти в папку с проектом и начать обучение модели.

- Установить TensorBoard для оценки результатов обучения и запустить.

![image](https://user-images.githubusercontent.com/92687732/205123455-139637bf-dde7-4431-b150-f103b9154a0b.png)

- Изначальный код:

```
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 1024
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```

- Изменим параметр batch-size на 2024.

```
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 2024
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```

![image](https://user-images.githubusercontent.com/92687732/205124238-61640e5b-058e-4a38-95fa-29b022a0284d.png)

Увеличение параметра batch-size привело к уменьшению средней награды, полученной агентом, на каждые 5000 шагов, что объясняется большим количеством тренировочных тестов на то же количество времени (количество эпох не изменилось).

- Изменим параметр hidden_units на 10.

```
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 1024
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 10
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```

![image](https://user-images.githubusercontent.com/92687732/205124947-71ba08d8-9854-4ccf-831a-12e7c64a957c.png)

Уменьшение параметра hidden_units привело к резкому скачку вверх награды при конце первого эпизода, засчёт меньшего количества нейронов, которые надо обучать, но это опять же скорее всего повлияло на качество обучения, снизив его. 

- Изменим параметр gamma на 0.5.

```
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 1024
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.5
        strength: 1.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```

![image](https://user-images.githubusercontent.com/92687732/205125211-d6243643-fe3f-4489-983f-6bc8b550769a.png)

Изменение параметра gamma в меньшую сторону сделало агента более сосредоточенным на получении краткосрочного вознаграждения, из-за чего средняя награда за итерацию претерпевала большие скачки не всегда в большую сторону, скорее всего этот параметр влияет на то, может ли агент уменьшить текущую награду, ради получения большей, но через какое-то количество шагов.

- Изменим параметр strength на 2.0.

```
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 1024
      buffer_size: 100
      learning_rate: 3.0e-4
      beta: 5.0e-4
      epsilon: 0.2
      lambd: 0.99
      num_epoch: 3
      learning_rate_schedule: linear
    network_settings:
      normalize: false
      hidden_units: 128
      num_layers: 2
    reward_signals:
      extrinsic:
        gamma: 0.99
        strength: 2.0
    max_steps: 500000
    time_horizon: 64
    summary_freq: 10000
```

![image](https://user-images.githubusercontent.com/92687732/205124748-9ff7d8c4-af08-47b2-b5eb-0ba85a2aea35.png)

Увеличение параметра strength привело к увеличению награды и, скорее всего, снижению качества обучения модели, так как потребовалось меньшее количество тестовых проходов для достижения того же результата.

## Задание 2
### Описать результаты, выведенные в TensorBoard.

-График Cumulative Reward: график, построенный на результатах среднего вознаграждения агентов в успешных эпизодах, по нему можно оценивать скорость обучения. 

-График Episode Length: соответствует количеству решений принятых агентом в течение одного эпизода

-График Policy Loss: Средняя величина функции потерь. Соответствует тому, насколько сильно меняется политика (процесс принятия решений о действиях). Этот график должен уменьшаться во время успешной тренировки.

-График Value Loss: Средняя потеря функции обновления значения. Соответствует тому, насколько хорошо модель способна предсказать значение каждого состояния. Этот график должен увеличиваться, пока агент учится, а затем уменьшаться, как только вознаграждение стабилизируется.

Соответственно с помощью графиков можно понять, какие параметры стоит изменить или оставить для лучшей тренировки модели.

## Выводы

В данной работе я научилась интегрировать экономическую систему в Unity и обучила ей пользоваться Ml Agent. Поработала с параметрами в .yaml файле: меняла значения для изменения обучения модели. Для поиска наилучшего обучения можно эксперементировать с параметрами в .yaml файле и при этом анализировать графики.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

# Разработка системы машинного обучения
Отчет по лабораторной работе #2 выполнил(а):
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
Познакомиться с программными средствами для создания системы машинного обучения и ее интеграции в Unity.

## Задание 1
### Реализовать систему машинного обучения в связке Python - Google-Sheets – Unity.

Ход работы:
- Создать новый пустой 3D проект на Unity.

![image](https://user-images.githubusercontent.com/92687732/198295682-3c5c55eb-a033-4e98-8f41-a7260bcd5efe.png)

- Скачать папку с ML агентом.

![image](https://user-images.githubusercontent.com/92687732/198295843-ddaa6561-30a1-4a0a-81c3-dcced6e9fa72.png)

- В созданный проект добавить ML Agent, выбрав Window - Package Manager - Add Package from disk. Последовательно добавить .json – файлы:

·	ml-agents-release_19 / com,unity.ml-agents / package.json

·	ml-agents-release_19 / com,unity.ml-agents.extensions / package.json

![image](https://user-images.githubusercontent.com/92687732/198296158-dcc552af-28f4-4096-bd8b-6d2a3971937a.png)

-	Все сделано правильно, поэтому во вкладке с компонентами (Components) внутри Unity появилась строка ML Agent.

![image](https://user-images.githubusercontent.com/92687732/198296876-4dd484eb-f4e7-4cfb-98e5-6b22b170bccd.png)

-	Далее запустить Anaconda Prompt для возможности запуска команд через консоль.

![image](https://user-images.githubusercontent.com/92687732/198297056-d6e6adc5-6ec6-41dc-b20a-4b2466168595.png)

- Далее написать серию команд для создания и активации нового ML-агента, а также для скачивания необходимых библиотек:

·	mlagents 0.28.0;

·	torch 1.7.1;

![image](https://user-images.githubusercontent.com/92687732/198297233-3224905f-f116-4374-8bb1-192f32e90cf9.png)

![image](https://user-images.githubusercontent.com/92687732/198297280-7f1e59b8-8e19-458d-af96-c9e24914827d.png)

![image](https://user-images.githubusercontent.com/92687732/198297331-3b0477a6-2391-444f-a4d5-afe1b5b513a3.png)

![image](https://user-images.githubusercontent.com/92687732/198297406-0e77f139-4bae-439d-83b3-ada2f69116fa.png)

![image](https://user-images.githubusercontent.com/92687732/198297462-3f73a02f-7ca7-4d30-b0fd-a6ae546527cb.png)

- Создать на сцене плоскость, куб и сферу так, как показано на рисунке ниже. Создать простой C# скрипт-файл и подключить его к сфере:

![image](https://user-images.githubusercontent.com/92687732/198297832-2bd94f37-d84f-40dc-b5ab-e45f7c28b44c.png)

- В скрипт-файл RollerAgent.cs добавить код.

![image](https://user-images.githubusercontent.com/92687732/198297991-e5758d63-d18c-42b5-89a0-653f4ab66198.png)

Код:

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }

    public Transform Target;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);

        if(distanceToTarget < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```
- Объекту «сфера» добавить компоненты Decision Requester, Behavior Parameters и настроить их.

![image](https://user-images.githubusercontent.com/92687732/198298602-f66fc3e0-d9ad-486b-8a67-08d748db622d.png)

- В корень проекта добавить файл конфигурации нейронной сети.

![image](https://user-images.githubusercontent.com/92687732/198298816-d1a091d8-a662-40f5-a85c-a3b2d0b900e3.png)

Код:

```
behaviors:
  RollerBall:
    trainer_type: ppo
    hyperparameters:
      batch_size: 10
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

- Запустить работу ml-агента.

![image](https://user-images.githubusercontent.com/92687732/198299906-271a53f6-7165-461c-959a-b3a9b16a8da1.png)

- Вернуться в проект Unity, запустить сцену, проверить работу ML-Agent’a.

![image](https://user-images.githubusercontent.com/92687732/198303393-80b373b5-b837-4fa9-ae88-105bd3ecfb19.png)

![image](https://user-images.githubusercontent.com/92687732/198303453-d0510b67-16cc-4d07-876c-a97d2b7b6fec.png)

![image](https://user-images.githubusercontent.com/92687732/198303499-e133d72d-31d0-4767-9139-6c8778f954f5.png)

- Сделать 3, 9, 27 копий модели «Плоскость-Сфера-Куб», запустить симуляцию сцены и наблюдать за результатом обучения модели.

1. 3 копии:

![image](https://user-images.githubusercontent.com/92687732/198304476-69120e8d-17c7-441f-bb21-81b8c0bb81dc.png)

![image](https://user-images.githubusercontent.com/92687732/198304594-502bd11b-9e06-488e-8491-e79ceec4460c.png)

![image](https://user-images.githubusercontent.com/92687732/198305814-a9e59752-e08b-4999-b52a-9c0c18ddf5e6.png)

2. 9 копий:

![image](https://user-images.githubusercontent.com/92687732/198305200-562fe92e-1073-457c-8110-048c3eff50da.png)

![image](https://user-images.githubusercontent.com/92687732/198305644-7c0ad999-dcd1-4943-98a1-ba5d28bb6733.png)

![image](https://user-images.githubusercontent.com/92687732/198306155-d70489af-053c-4ddf-8bd6-8e53fd779a62.png)

3. 27 копий:

![image](https://user-images.githubusercontent.com/92687732/198313906-c0cc7aeb-0dc5-4d60-929d-1526fb941b3f.png)

![image](https://user-images.githubusercontent.com/92687732/198314026-c1435899-59e8-4dd4-bf09-eab0a0078e97.png)

![image](https://user-images.githubusercontent.com/92687732/198307089-0eaa1465-ae1d-4390-a097-3a31d32d2172.png)

Вывод: После обучения модели видно, что шарик стал двигаться напрямую к кубику, перестал постоянно падать за пределы платформы и стал быстрее находить кубик.

## Задание 2
### Подробно описать каждую строку файла конфигурации нейронной сети. Самостоятельно найти информацию о компонентах Decision Requester, Behavior Parameters, добавленных сфере.

Код с описанием:

```
behaviors:
 RollerBall: #Имя агента
    trainer_type: ppo                #Устанавливаем режим обучения (Proximal Policy Optimization).
    hyperparameters: #Задаются гиперпараметры.
      batch_size: 10                 #Количество опытов на каждой итерации для обновления экстремумов функции.
      buffer_size: 100               #Количество опыта, которое нужно набрать перед обновлением модели.
      learning_rate: 3.0e-4          #Устанавливает шаг обучения (начальная скорость).
      beta: 5.0e-4                   #Отвечает за случайность действия, повышая разнообразие и иследованность пространства обучения.
      epsilon: 0.2                   #Порог расхождений между старой и новой политиками при обновлении.
      lambd: 0.99                    #Определяет авторитетность оценок значений во времени. Чем выше значение, тем более авторитетен набор предыдущих оценок.
      num_epoch: 3                   #Количество проходов через буфер опыта, при выполнении оптимизации.
      learning_rate_schedule: linear #Определяет, как скорость обучения изменяется с течением времени, линейно уменьшает скорость.
    network_settings: #Определяет сетевые настройки.
      normalize: false               #Отключается нормализация входных данных.
      hidden_units: 128              #Количество нейронов в скрытых слоях сети.
      num_layers: 2                  #Количество скрытых слоев для размещения нейронов.
    reward_signals: #Задает сигналы о вознаграждении.
      extrinsic:
 gamma: 0.99                  #Коэффициент скидки для будущих вознаграждений.
        strength: 1.0                #Шаг для learning_rate.
    max_steps: 500000                #Общее количество шагов, которые должны быть выполнены в среде до завершения обучения.
    time_horizon: 64                 #Количество циклов ML агента, хранящихся в буфере до ввода в модель.
    summary_freq: 10000              #Количество опыта, который необходимо собрать перед созданием и отображением статистики.
```

- Decision Requester - запрашивает решение через регулярные промежутки времени и обрабатывает чередование между ними во время обучения.

- Behavior Parameters - определяет принятие объектом решений, в него указывается какой тип поведения будет использоваться: уже обученная модель или удалённый процесс обучения.

## Задание 3
### Доработать сцену и обучить ML-Agent таким образом, чтобы шар перемещался между двумя кубами разного цвета. Кубы должны, как и впервом задании, случайно изменять кооринаты на плоскости. 

- Создадим еще один куб.

![image](https://user-images.githubusercontent.com/92687732/198314438-876a8717-61e7-4d44-957c-e4618a27fcdb.png)

- Обновим код в скрипте, чтобы у шара появилась новая цель (новый кубик).

Код: 

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Unity.MLAgents;
using Unity.MLAgents.Sensors;
using Unity.MLAgents.Actuators;

public class RollerAgent : Agent
{
    Rigidbody rBody;
    // Start is called before the first frame update
    void Start()
    {
        rBody = GetComponent<Rigidbody>();
    }
    public Transform Target;
    public Transform Target2;
    public override void OnEpisodeBegin()
    {
        if (this.transform.localPosition.y < 0)
        {
            this.rBody.angularVelocity = Vector3.zero;
            this.rBody.velocity = Vector3.zero;
            this.transform.localPosition = new Vector3(0, 0.5f, 0);
        }

        Target.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
        Target2.localPosition = new Vector3(Random.value * 8-4, 0.5f, Random.value * 8-4);
    }
    public override void CollectObservations(VectorSensor sensor)
    {
        sensor.AddObservation(Target.localPosition);
        sensor.AddObservation(this.transform.localPosition);
        sensor.AddObservation(rBody.velocity.x);
        sensor.AddObservation(rBody.velocity.z);
    }
    public float forceMultiplier = 10;
    public override void OnActionReceived(ActionBuffers actionBuffers)
    {
        Vector3 controlSignal = Vector3.zero;
        controlSignal.x = actionBuffers.ContinuousActions[0];
        controlSignal.z = actionBuffers.ContinuousActions[1];
        rBody.AddForce(controlSignal * forceMultiplier);

        float distanceToTarget = Vector3.Distance(this.transform.localPosition, Target.localPosition);
        float distanceToTarget2 = Vector3.Distance(this.transform.localPosition, Target2.localPosition);

        if(distanceToTarget < 1.42f // distanceToTarget2 < 1.42f)
        {
            SetReward(1.0f);
            EndEpisode();
        }
        else if (this.transform.localPosition.y < 0)
        {
            EndEpisode();
        }
    }
}
```
- Запустим.

![image](https://user-images.githubusercontent.com/92687732/198312030-cafa9477-735e-46fb-9ae6-9e7cff0f25df.png)

![image](https://user-images.githubusercontent.com/92687732/198312164-ef935a78-0ba4-49bc-b9cb-4b84803adfe5.png)

## Выводы

  В данной работе я познакомилась с программными средствами для создания системы машинного обучения и ее интеграции в Unity. Изучила, что такое игровой баланс и могу сказать, что это - субъективное «равновесие» между персонажами, командами, тактиками игры и другими игровыми объектами. Баланс должен быть и в сюжете, и сложности игры. Для настройки баланса может потребоваться достаточно много времени, поэтому существуют системы машинного обучения, которые помогут поддерживать этот баланс. Машинное обучение позволяет более точно распределять игровые механики, подлежащие балансу.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

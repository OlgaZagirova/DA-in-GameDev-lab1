# АНАЛИЗ ДАННЫХ И ИСКУССТВЕННЫЙ ИНТЕЛЛЕКТ [in GameDev]
Отчет по лабораторной работе #1 выполнил(а):
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
Ознакомиться с основными операторами зыка Python на примере реализации линейной регрессии.

## Задание 1
### Написать команды Hello World на Python и Unity.
Ход работы:
- Для Python в отчете привести скриншоты с демонстрацией сохранения документа google.colab на свой диск с запуском программы, выводящей сообщение Hello World.

![image](https://user-images.githubusercontent.com/92687732/192148116-e85080de-92de-4165-ad79-b2514251b253.png)

![image](https://user-images.githubusercontent.com/92687732/192148132-af44887c-2ff1-4613-935a-4a2205a30bbf.png)

- Для Unity в отчете привести скриншот вывода сообщения Hello World в консоль.

![image](https://user-images.githubusercontent.com/92687732/192148224-539ab4d4-bbf0-4c01-b1b6-9d5330ce00e0.png)

## Задание 2
### В разделе "Ход работы" пошагово выполнить каждый пункт с описанием и примером реализации задачи по теме лабораторной работы
Ход работы:

- 1. Произвести подготовку данных для работы с алгоритмом линейной регрессии. 10 видов данных были установлены случайным образом, и данные находились в линейной зависимости. Данные проебразуются в формат массива, чтобы их можно было вычислить напрямую при использовании умножения и сложения.

![image](https://user-images.githubusercontent.com/92687732/192148431-91e3847f-9626-4dca-9175-95f3d1f36c7b.png)

- 2. Определите связанные функции. Функция модели: определяет модель линейной регрессии wx+b. Функция потерь: функция потерь среднеквадратичной ошибки. Функция оптимизации: метод градиентного спуска для нахождения частных производных w и b.

![image](https://user-images.githubusercontent.com/92687732/192148485-f16e4674-ec43-41b4-9ba0-0de175bb71b4.png)

- 3. Начать итерацию

Шаг 1. Инициализация и модель итеративной оптимизации

![image](https://user-images.githubusercontent.com/92687732/192148567-12c466ae-c2f3-43d8-beb2-03c34c49607f.png)

Шаг 2. На второй итерации отображаются значения параметров, значения потерь и эффекты визуализации после итерации

![image](https://user-images.githubusercontent.com/92687732/192148602-d65a38c2-9a7a-4448-8516-7682c6545133.png)

Шаг 3. Третья итерация показывает значения параметров, значения потерь и визуализацию после итерации

![image](https://user-images.githubusercontent.com/92687732/192148622-80d5912c-e549-4b2b-aa6c-347da5fd28d4.png)

Шаг 4. На четвертой итерации отображаются значения параметров, значения потерь и эффекты визуализации

![image](https://user-images.githubusercontent.com/92687732/192148637-f70e02ba-0823-438c-9556-1f94ae3f41f1.png)

Шаг 5. Пятая итерация показывает значение параметра, значение потерь и эффект визуализации после итерации

![image](https://user-images.githubusercontent.com/92687732/192148690-9e766514-9ede-407a-b3e5-87f001aae3f9.png)

Шаг 6. 10000-я итерация, показывающая занчения параметров, потери и визуализацию после итерации

![image](https://user-images.githubusercontent.com/92687732/192148711-0d672d6e-5a6f-47bd-a9a9-e8710e33406d.png)

## Задание 3
### Изучить код на Python и ответить на вопросы:

- Должна ли величина loss стремиться к нулю при изменении исходных данных? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ.

Величина loss должна стремиться к нулю при изменении исходных данных, так как данная величина указывает на количество потерь. При увеличении количества итерации эффективность алгоритма растет, а количество потерь снижается. Рассмотрим значения на примере увеличения итерации из 2 задания 1-6 шаги.

- Какова роль параметра Lr? Ответьте на вопрос, приведите пример выполнения кода, который подтверждает ваш ответ. В качестве эксперимента можете изменить значение параметра.

Параметр Lr является коэффициентом скорости обучения. Чем больше значение Lr, тем быстрее переменная loss стремиться к нулю, так как эффективность алгоритма становится выше, а количество потерь уменьшается.

- 0.1
![image](https://user-images.githubusercontent.com/92687732/192152323-27bf61e6-a6f7-4a8e-bf44-a9f586db3b20.png)

- 0.001
![image](https://user-images.githubusercontent.com/92687732/192152348-a85b6c03-67bd-4365-8487-2c3230be8ed4.png)

- 0.0001
![image](https://user-images.githubusercontent.com/92687732/192152366-b05374dc-f779-4513-bebb-dee330287500.png)

## Выводы

В данной работе я ознакомилась с основными операторами зыка Python на примере реализации линейной регрессии. Вывела в консоль Unity и Google Colab текст "Hello World!". Установила такие ПО как: Unity и Anacondа.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

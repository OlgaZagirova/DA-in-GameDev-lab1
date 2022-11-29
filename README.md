# Перцептрон
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
Ознакомиться с работой перцептрона.

## Задание 1
### В проекте Unity реализовать перцептрон, который умеет производить вычисления.

Ход работы:

- Добавить пустой объект и подключить к нему скрипт Perceptron

![image](https://user-images.githubusercontent.com/92687732/204614113-eba0e8ff-50fe-41f2-b854-52052151bf77.png)

Код: 

```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class TrainingSet
{
	public double[] input;
	public double output;
}

public class Perceptron : MonoBehaviour {

	public TrainingSet[] ts;
	double[] weights = {0,0};
	double bias = 0;
	double totalError = 0;

	double DotProductBias(double[] v1, double[] v2) 
	{
		if (v1 == null || v2 == null)
			return -1;
	 
		if (v1.Length != v2.Length)
			return -1;
	 
		double d = 0;
		for (int x = 0; x < v1.Length; x++)
		{
			d += v1[x] * v2[x];
		}

		d += bias;
	 
		return d;
	}

	double CalcOutput(int i)
	{
		double dp = DotProductBias(weights,ts[i].input);
		if(dp > 0) return(1);
		return (0);
	}

	void InitialiseWeights()
	{
		for(int i = 0; i < weights.Length; i++)
		{
			weights[i] = Random.Range(-1.0f,1.0f);
		}
		bias = Random.Range(-1.0f,1.0f);
	}

	void UpdateWeights(int j)
	{
		double error = ts[j].output - CalcOutput(j);
		totalError += Mathf.Abs((float)error);
		for(int i = 0; i < weights.Length; i++)
		{			
			weights[i] = weights[i] + error*ts[j].input[i]; 
		}
		bias += error;
	}

	double CalcOutput(double i1, double i2)
	{
		double[] inp = new double[] {i1, i2};
		double dp = DotProductBias(weights,inp);
		if(dp > 0) return(1);
		return (0);
	}

	void Train(int epochs)
	{
		InitialiseWeights();
		
		for(int e = 0; e < epochs; e++)
		{
			totalError = 0;
			for(int t = 0; t < ts.Length; t++)
			{
				UpdateWeights(t);
				Debug.Log("W1: " + (weights[0]) + " W2: " + (weights[1]) + " B: " + bias);
			}
			Debug.Log("TOTAL ERROR: " + totalError);
		}
	}

	void Start () {
		Train(8);
		Debug.Log("Test 0 0: " + CalcOutput(0,0));
		Debug.Log("Test 0 1: " + CalcOutput(0,1));
		Debug.Log("Test 1 0: " + CalcOutput(1,0));
		Debug.Log("Test 1 1: " + CalcOutput(1,1));		
	}
	
	void Update () {
		
	}
}
```

- Заполнить каждый элемент.

1. OR, понадобилось 3 эпохи, 4 вычисляла корректно.

![image](https://user-images.githubusercontent.com/92687732/204615507-6d2b48eb-0d7a-4a99-980c-42a964cce8ed.png)

![image](https://user-images.githubusercontent.com/92687732/204615435-bea2e002-d910-40d0-8870-a7bf51d3fb12.png)

2. AND, понадобилось 5 эпох, 6 вычисляла корректно.

![image](https://user-images.githubusercontent.com/92687732/204616350-51a99da3-8541-4987-85ca-fd198466d1ca.png)

![image](https://user-images.githubusercontent.com/92687732/204615964-6b2d6e85-cbd8-4994-b88b-b8b8d80136be.png)

3. NAND, понадобилось 5 эпох, 6 вычисляла корректно.

![image](https://user-images.githubusercontent.com/92687732/204616350-51a99da3-8541-4987-85ca-fd198466d1ca.png)

![image](https://user-images.githubusercontent.com/92687732/204617423-88df3cef-3c2d-4173-a900-99ece37d0a90.png)

4. XOR, даже спустя 100000 эпох не смог обучиться и поэтому некорректно выполняет вычисления.

![image](https://user-images.githubusercontent.com/92687732/204619026-2c58e820-d31c-4b47-a66b-06332309fc50.png)

![image](https://user-images.githubusercontent.com/92687732/204618716-59e54237-38c8-4542-ace7-a4ccb18e1810.png)

## Задание 2
### Построить графики зависимости количества эпох от ошибки обучения. Указать от чего зависит необходимое количество эпох обучения.

Ход работы:

- Внести данные в excel и построить графики.

![image](https://user-images.githubusercontent.com/92687732/204627993-a2ab9ff7-e24e-49fb-9cbb-dcd4c7c78bf4.png)

- Количество эпох обучения зависит от смещения(weights) и веса(bias).

![image](https://user-images.githubusercontent.com/92687732/204631551-a913c998-d2ff-4300-9d1d-337619ed1847.png)

## Задание 3
### Построить визуальную модель работы перцептрона на сцене Unity.

- Создадим модель для работы XOR, белые кубы - нули, черные - единицы

![image](https://user-images.githubusercontent.com/92687732/204641411-6ed94bc9-7513-4b3d-8e2c-f80fed322e03.png)

Код для изменения цвета при столкновении: 

```
using UnityEngine;

public class ChangeColor : MonoBehaviour
{
    private void OnTriggerEnter(Collider other)
    {
        if (other.gameObject.GetComponent<Renderer>().material.color == Color.black && this.gameObject.GetComponent<Renderer>().material.color == Color.black)
        {
            other.gameObject.GetComponent<Renderer>().material.color = Color.white;
            this.gameObject.GetComponent<Renderer>().material.color = Color.white;
        }
        else if (other.gameObject.GetComponent<Renderer>().material.color == Color.black && this.gameObject.GetComponent<Renderer>().material.color == Color.white)
        {
            other.gameObject.GetComponent<Renderer>().material.color = Color.black;
            this.gameObject.GetComponent<Renderer>().material.color = Color.black;
        }
        else if (other.gameObject.GetComponent<Renderer>().material.color == Color.white && this.gameObject.GetComponent<Renderer>().material.color == Color.black)
        {
            other.gameObject.GetComponent<Renderer>().material.color = Color.black;
            this.gameObject.GetComponent<Renderer>().material.color = Color.black;
        }
        else if (other.gameObject.GetComponent<Renderer>().material.color == Color.white && this.gameObject.GetComponent<Renderer>().material.color == Color.white) {
            other.gameObject.GetComponent<Renderer>().material.color = Color.white;
            this.gameObject.GetComponent<Renderer>().material.color = Color.white;
        }
    }
}
```
- Запустим.

![image](https://user-images.githubusercontent.com/92687732/204645034-53cf1f11-e352-41db-be60-be2c2b5e2cd6.png)

![image](https://user-images.githubusercontent.com/92687732/204645138-ff4c2233-8ec1-4d7d-aeec-1ccd1e33b945.png)

![image](https://user-images.githubusercontent.com/92687732/204645184-399340cf-24b7-4785-8934-5a706fc34143.png)

![image](https://user-images.githubusercontent.com/92687732/204645237-f6338b3b-93a1-455d-843e-1d133b8d54a0.png)

## Выводы

  В данной работе я ознакомилась с работой перцептрона, который вычисляет различные функции, построила графики зависимости количества эпох от ошибки обучения, указала от чего зависит необходимое количество эпох обучения, построила визуальную модель работы перцептрона на сцене Unity.

## Powered by

**BigDigital Team: Denisov | Fadeev | Panov**

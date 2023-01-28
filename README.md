# Итоговая проверочная работа 1-го семестра

## Цель: 
Данная работа необходима для проверки знаний и навыков по итогу прохождения первого блока обучения по программе _**Разработчик**_. По итогам работы будет сделан вывод об успешности базового знакомсва с IT.

## Необходимые условия:
Для полноценного выполнения проверочной работы необходимо:
1. Создать репозиторий на GitHub
2. Нарисовать блок-схему алгоритма (достаточно блок-схемы основной содержательной части, если она выделяется в отдельный метод)
3. Снабдить репозиторий оформленным текстовым описанием решения (файл README.md)
4. Написать программу, решающую поставленную задачу
5. Использовать контроль версий в работе над этим небольшим проектом (не должно быть так, что все залито одним коммитом, как минимум этапы 2,3 и 4 должны быть расположены в разных коммитах).

## Задача:
Написать программу, которая из имеющегося массива строк формирует массив строк, длина которых меньше либо равна 3 символа. Первоначальный массив можно ввести с клавиатуры, либо задать на старте выполнения алгоритма. При решении не рекомендуется пользоваться коллекциями, лучше обойтись исключительно массивами.

## Примеры:
>["hello","2","world",":-)"] -> ["2",":-)"]

>["1234","1567","-2","computer science"] -> ["-2"]

>["Russia","Denmark","Kazan"] -> []

## Описаение решения:
Работу программы можно разделить на три крупных этапа:
1. Ввод массива пользователем;
2. Создание нового массива, состоящего из элементов, удовлетворяющих заданному условию;
3. Вывод созданного массива.

Рассмотрим этапы работы более подробно.

### Ввод массива пользователем:
В условии задачи длина массива строк не регламентирована и, следовательно, может быть любой. Поэтому, в первую очередь нужно спросить пользователя про желаемое количество элементов массива строк (для простоты будем называть их словами). Для получения данных из консоли создадим метод, который будет выводить в консоль нужную нам фразу и возвращать то, что ввел пользователь:
```c#
string TakeData (string msg) { 
    Console.Write(msg); //выводим полученное сообщение
    return Console.ReadLine()??"ERROR"; //возвращаем введенное пользователем слово
    }
```
Данный метод возвращает данные типа _**string**_ так как мы будем использовать его для наполнения массива строк. В случае использования этого метода для получения целых чисел потребуется воспользоваться преобразованием типов. Создадим переменную _**numWords**_ и считаем в неё количество слов для массива строк:
```c#
int numWords=int.Parse(TakeData("Введите количество слов: "));
```
Количесво слов нам известно. Теперь создадим массив требуемой длинны:
```c#
string [] inData=new string[numWords];
```
Для наполнения массива словами воспользуемся циклом _**while**_ и ранее созданным методом _**TakeData**_

``` c#
while (i<numWords){  
    inData[i]=TakeData($"Введите {i+1}-е слово: ");
    i++;
}
```
### Создание нового массива по условию:
По условию задачи результатом работы программы должен быть массив, состоящий из элементов исходного массива, длина которых меньше или равна 3. Основной проблеммой является то, что _**C#**_ не поддерживает динамические массивы. Длина массива в _**C#**_ всегда строго определяется в момент его инициализации и может быть изменена  только перезаписью в новый массив.

Рассмотрим варианты реализации алгоритма решения задачи:
* Вариант 1:
    * Просматриваем массив слов и считаем количество элементов длина которых <= 3. 
    * Затем создаем массив уже известной длинны и наполняем его повтороным проходом массива слов.

* Вариант 2:
    * Создаем массив длина которого соответствует длине массива слов. 
    * Наполняем этот массив элементами из массива слов, удовлетворяющих условию.
    * "Отрезаем" лишние элементы массива.

При написании программы будем использовать **Вариант 2**, т.к. первый вариант является неоптимальным из-за необходимости двойного прохода по массиву слов.

Для изменения длинны массива воспользуемся стандартным методом:
``` c#
Array.Resize(T[], lengh)
```
где: **T[]** - передаваемый массив для изменения длинны;\
    &emsp;&emsp;**lengh** - переменная типа Int32, содержащая информацию о новой длинне массива. 

Итоговый код метода, создающего новый массив, будет выглядеть:

```c#
string [] LessThen (string [] inArray, int amount){ // метод принимающий массив строк и количество символов и возвращающий массив, состоящий из элементов переданного массива, короче заданной длинны
    int arrayLength = inArray.Length; // переменная содержащая длину массива, чтоб не высчитывать её много раз
    string [] result = new string[arrayLength]; // создаем массив длинной равной полученному массиву для сбора результата
    int i=0; // переменная счетчик для входящего массива
    int j=0; // переменная счетчик для массива результата

    while (i<arrayLength){ // цикл по полученному массиву        
        if (inArray[i].Length<=amount) {
            result[j]=inArray[i]; // если длина текущего элемента полученного массива <= полученного значения, то записываем этот элемент в результат
            j++; // переходим к следующему элементу результирующего массива
        }
        i++; // переходим к следующему элементу полученного массива
    }
    
    Array.Resize(ref result,j); // отрезаем лишние элементы от результирующего массива     
    return result;
}
```
### Вывод созданного массива

Для демонстрации результата работы программы создадим метод, выводящи в консоль переданный одномерный массив. Вывод будем осуществлять по следующей маске:

> _["Элемент №0", "Элемент #1",...]_

Метод также должен отрабатывать вариант вывода пустого массива. Поэтому первым шагом метода будет расчет длинны массива. После чего запишем в консоль:
 >_[ "0-й элемент массива_ 
 
 если методу передали массив ненулевой длинны, либо:
 > _**[**_ 
 
 если был получен пустой массив.

Следующим этапом запускаем цикл _**while**_ по массиву, который допишет оставшиеся элементы. Начальное значение переменной-счетчика примем равным **1**, поскольку 0-й элемент массива уже выведен в консоль. В случае передачи методу пустого массива в тело цикла мы не попадем, так как переменная-счетчик будет больше длинны массива.

После завершения цикла необходимо закрыть вывод информации дописав: 

> _"]_

если методу был передан не пустой массив, либо:

> _]_

если был получен массив нулевой длинны.

Итоговый код метода вывода массива имеет вид:
``` c#
void PrintArray (string [] inArray){ // метод выводящий одномерный массив на экран
    int i=1; // переменная счетчик
    int arrayLength = inArray.Length; // переменная содержащая длину массива, чтоб не высчитывать её много раз 

    if (arrayLength!=0) Console.Write("[\""+inArray[0]); // если массив не пустой, то выводим 0-й элемент с элементами оформления вывода
    else Console.Write("["); // иначе выводим только открывющуюся скобку

    while (i<arrayLength){ 
        Console.Write("\",\""+inArray[i]); // выводим i-й элемент массива
        i++;
    }

    if (arrayLength!=0) Console.WriteLine("\"]"); // если массив не пустой, то закрываем кавычку и квадратную скобку
    else Console.WriteLine("]"); // иначе только закрываем скобку
    }
```


### Вызов методов

После ввода слов пользователем ([см. Этап 1](#ввод-массива-пользователем:)) выводим в консоль информацию для пользователя и обращаемся к написанным нами методам для расчета массива и вывода результата в консоль:

```c#
Console.WriteLine("\r\nМассив элементов длинной <=3:");
PrintArray(LessThen(inData,3));
```


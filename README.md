![FEFU logo](https://user-images.githubusercontent.com/98378287/218238526-76472004-13bf-41c4-b203-9b1503e71812.png)
## Федеральное агентство по образованию РФ
## Дальневосточный федеральный университет
## ИНСТИТУТ МАТЕМАТИКИ И КОМПЬЮТЕРНЫХ ТЕХНОЛОГИЙ
## Департамент математического и компьютерного моделирования
# A* pathfinding algorithm
## Доклад
### Студента гр. Б9121-09.03.03пикд
### Шкарина Дарья Евгеньевна.
### Руководитель
### Кленин А. С.
### г. Владивосток 2023

____

# Cодержание <a name="Cодержание"></a>

- <a href = "#1">Глоссарий</a>
- <a href = "#2">Введение</a>
- <a href = "#3">Постановка задачи</a>
- <a href = "#4">Авторы и история</a>
- <a href = "#5">Взвешенные графы</a>
- <a href = "#6">Описание алгоритма</a>
  - <a href = "#7">Особые случаи</a>
  - <a href = "#8">Эвристические алгоритмы</a>
- <a href = "#9">Оценка сложности алгоритма</a>
  - <a href = "#10">Оценка сложности алгоритма A star</a>
- <a href = "#11">Тестирование и анализ </a>
- <a href = "#12">Заключение</a>
- <a href = "#13">Литература</a>

____


# Глоссарий<p id = "1"></p>

1. `Алгоритм Дейкстры` это алгоритм на графах, изобретённый нидерландским учёным Эдсгером Дейкстрой в 1959 году. находит кратчайшие пути от заданной вершины  до всех остальных в графе без ребер отрицательного веса. 
2. `Эвристическая функция` это функция, которая не выведена математически строго, а придумана на основании каких-то соображений.
3. `Взвешенный граф` это граф, в котором у каждого ребра и/или каждой вершины есть “вес” - некоторое число, которое может обозначать длину пути, его стоимость и т. п.
4. `Оптимальность` алгоритма -  свойство алгоритма, гарантирующее получение **лучшего из возможных решений**.
5. `Эвристический алгоритм`  это алгоритм решения задачи, правильность которого для всех возможных случаев не доказана, но про который известно, что он даёт достаточно хорошее решение в большинстве случаев.
6. `Поиск пути (англ. Pathfinding)` это термин в информатике и искусственном интеллекте, который означает определение компьютерной программой наилучшего, оптимального маршрута между двумя точками.
7. `Сложность алгоритма` это количественная характеристика, которая говорит о том, сколько времени, либо какой объём памяти потребуется для выполнения алгоритма.

____


# Введение <p id = "2"></p>

 _A star ( A*) алгоритм_ -  алгоритм поиска по первому наилучшему совпадению на графе, который находит маршрут с наименьшей стоимостью от начальной вершины к целевой.

![AStar](https://user-images.githubusercontent.com/98378287/206121400-d5dc0ce7-b0b8-49c0-b222-14683123e79f.gif)

 
Основная разница между Алгоритмом Дейкстры и A* заключается в том что алгоритм Дейкстры находит кратчайшие пути для всех точек на графе, в то время как А* оптимизирован для нахождения кратчайшего пути до одной точки, что позволяет существенно сократить время работы алгоритма.

Алгоритм А* находит оптимальный вариант благодаря вычислению суммарной стоимости всех путей между начальной и конечной точкой. Этот способ считается быстрее алгоритма Дейкстры благодаря эвристической функции.

Алгоритм А* обладает двумя ключевыми характеристиками алгоритмов такого рода: **оптимальность** и **полнота**[2].

А* идеально подходит для любой задачи где нужно найти кратчайшее расстояние к конечной точке, и из-за этого алгоритм оказался очень хорошо заточен для компьютерных игр, где часто используется.

Еще один аспект, делающий A* таким мощным, — это использование в его реализации взвешенных графов. Взвешенный граф использует числа для представления стоимости каждого пути или курса действий. Это означает, что алгоритмы могут выбрать путь с наименьшими затратами и найти лучший маршрут с точки зрения расстояния и времени.

A* пошагово просматривает все пути, ведущие от начальной вершины в конечную, пока не найдёт минимальный.
____
 
# Постановка задачи <p id = "3"></p>

В ходе реализации данного алгоритма необходимо:

- Изучить описание, свойства, особые функции и тонскости реализации алгоритма А*
- Осуществить реализацию алгоритма как отдельного header-файла
- Выполнить тестирование на корректность и сложность работы алгоритма, отобразить графически

____

# Авторы и история <p id = "4"></p>

Алгоритм А* — один из самых эффективных алгоритмов поиска кратчайшего пути между двумя точками графа. Он был опубликован Питером Хартом, Нильсом Нильссоном и Бертрамом Рафаэлем в 1968 году[1]. В каком-то смысле его можно назвать расширением алгоритма Дейкстры. Однако, несмотря на это он является одним из самых часто используемых алгоритмов поиска пути.

____
  
# Взвешенные графы <p id = "5"></p>

В классических графах все рёбра считаются равноценными и длина пути соответствует количеству рёбер, которые он содержит. Однако часто в задаче каждому ребру соответствует некоторый параметр - длина ребра или стоимость прохождения по нему. В терминологии графов такой параметр называется весом ребра, а граф, содержащий взвешенные рёбра, взвешенным.[12]

Взвешенный граф — это граф, в котором каждое ребро обозначается числом. Это число — его вес:

![взвешенный граф](https://user-images.githubusercontent.com/98378287/218616479-86f25378-2c5e-4570-91e6-efa4caf10365.png)

Вес — это стоимость использования данного ребра.


____
 
# Описание алгоритма <p id = "6"></p>

Эвристический алгоритм жертвует точностью и аккуратностью ради скорости, чтобы решать проблемы быстрее и эффективнее.

Все графы имеют разные узлы или точки, которые должен пройти алгоритм, чтобы достичь конечного узла. Все пути между этими узлами имеют числовое значение, которое считается весом пути. Сумма всех поперечных путей дает вам стоимость этого маршрута.

Первоначально алгоритм вычисляет стоимость для всех своих ближайших соседних узлов, n, и выбирает тот, который несет наименьшую стоимость. Этот процесс повторяется до тех пор, пока не будут выбраны новые узлы и не будут пройдены все пути. Затем вы должны рассмотреть лучший путь среди них. Если f(n) представляет окончательную стоимость, то ее можно обозначить как:

$$f(n) = g(n) + h(n),$$ где: $$g(n) =$$ стоимость перехода от одного узла к другому. Это будет варьироваться от узла к узлу $$h(n) =$$ эвристическая аппроксимация значения узла. 


Стоимость перехода от одной клетки к другой определяется несколькими значениями. 

Эвристические функции для алгоритма A*[5]:

- Если мы можем перемещаться только в 4 направлениях то в качестве эвристики используется Манхэттенское расстояние
$$H(v) = abs(v.x - goal.x) + abs(v.y - goal.y) $$

- Если к 4 направлениям доавляются диагонали то применяют расстояние Чебышева:
$$H(v) = max(abs(v.x - goal.x), abs(v.y - goal.y))$$

- Если передвижение не ограниченно сеткой, то можно использовать Евклидово расстояние до прямой:
$$H(v) = sqrt(((v.x - goal.x)**2 + abs(v.y - goal.y)**2)$$

Блок-схема алгоритма выглядит таким образом:

![блок-схема алгоритма](https://user-images.githubusercontent.com/98378287/218615281-d5c60270-4d82-47de-b70f-6c9d10158fe6.png)
____

## Особые случаи <p id = "7"></p>

В общем говоря, поиск в глубину и поиск в ширину являются двумя частными случаями алгоритма A*. Для поиска в глубину возьмём глобальную переменную-счётчик С, инициализировав её неким большим значением. Всякий раз при раскрытии вершины будем присваивать значение счётчика смежным вершинам, уменьшая его на единицу после каждого присваивания. Таким образом, чем раньше будет открыта вершина, тем большее значение h(x) она получит, а значит, будет просмотрена в последнюю очередь. Если положить h(x) = 0 для всех вершин, то мы получим ещё один специальный случай — алгоритм Дейкстры.

____

## Эвристические алгоритмы <p id = "8"></p>

Эвристический алгоритм — практический метод решения задачи, не всегда гарантированно приводящий к точным или оптимальным результатам, но позволяющий ускорить выполнение поставленного задания[3].

Эвристические алгоритмы широко применяются для решения задач высокой вычислительной сложности, то есть вместо полного перебора вариантов, занимающего существенное время, а иногда технически невозможного, применяется значительно более быстрый, но недостаточно теоретически обоснованный алгоритм. В областях искусственного интеллекта, таких как распознавание образов, эвристические алгоритмы широко применяются также и по причине отсутствия общего решения поставленной задачи. Различные эвристические подходы применяются в антивирусных программах, компьютерных играх и т. д.

 В действительности может быть даже известно (то есть доказано), что эвристический алгоритм формально неверен. Его всё равно можно применять, если при этом он даёт неверный результат только в отдельных, достаточно редких и хорошо выделяемых случаях или же даёт неточный, но всё же приемлемый результат.

Проще говоря, эвристика — это не полностью математически обоснованный (или даже «не совсем корректный»), но при этом практически полезный алгоритм.

Важно понимать, что эвристика, в отличие от корректного алгоритма решения задачи, обладает следующими особенностями[3]:

- Она не гарантирует нахождение лучшего решения;
- Она не гарантирует нахождение решения, даже если оно заведомо существует (возможен «пропуск цели»);
- Она может дать неверное решение в некоторых случаях.

____

# Оценка сложности алгоритма <p id = "9"></p>

В информатике временна́я сложность алгоритма определяется как функция от длины строки, представляющей входные данные, равная времени работы алгоритма на данном входе[8].

Основным показателем сложности алгоритма является время, необходимое для решения задачи и объём требуемой памяти.
Также при анализе сложности для класса задач определяется некоторое число, характеризующее некоторый объём данных – размер входа.
Итак, можем сделать вывод, что сложность алгоритма – функция размера входа.
Сложность алгоритма может быть различной при одном и том же размере входа, но различных входных данных.

Порядок роста сложности (или аксиоматическая сложность) описывает приблизительное поведение функции сложности алгоритма при большом размере входа. Из этого следует, что при оценке временной сложности нет необходимости рассматривать элементарные операции, достаточно рассматривать шаги алгоритма[7].

Шаг алгоритма – совокупность последовательно-расположенных элементарных операций, время выполнения которых не зависит от размера входа, то есть ограничена сверху некоторой константой.

Существуют понятия сложности в худшем, среднем или лучшем случае. Обычно, оценивают сложность в худшем случае.

____
 
 ## Оценка сложности алгоритма A star <p id = "10"></p>

Временна́я сложность алгоритма A* зависит от эвристики. В худшем случае, число вершин, исследуемых алгоритмом, растёт экспоненциально по сравнению с длиной оптимального пути, но сложность становится полиномиальной, когда эвристика удовлетворяет следующему условию:

$${|h(x)-h_1(x)|<= (O(logh_1(x))} $$

где h_1 — оптимальная эвристика, то есть точная оценка расстояния из вершины x к цели. Другими словами, ошибка h(x) не должна расти быстрее, чем логарифм от оптимальной эвристики[9].
____

# Тестирование и анализ <p id = "11"></p>

Для проверки корректности работы алгоритма было реализовано относительно небольшое количество ручных тестов ( N < 100), в каждом из которых проверялись различные ситуации. 

Для удобной оценки сложности алгоритма на вход подается 100 автоматических тестов, которые состоят из матриц размерности $$n^2$$. После проверки которых были получены следующие данные:
![оценка сложности алгоритма](https://user-images.githubusercontent.com/98378287/218610087-467e232c-2858-4e98-9b49-afc3390212b4.png)


Как и было описанно выше, сложность алгоритма растет экспоненциально, что удовлетворяет ожидаемому результату.
____

# Заключение <p id = "12"></p>
 
- Изучить материалы по алгоритму A star
- Выполнить реализацию алгоритма на языке c++
- Выполнить иследование алгоритма на производительность

____

# Литература <p id = "13"></p>

- [1].https://ru.wikipedia.org/wiki/A*
- [2].https://medium.com/nuances-of-programming/алгоритм-поиска-a-3bb59be05a79
- [3].https://studfile.net/preview/2959481/page:16/
- [4].https://ru.wikipedia.org/wiki/Эвристический_алгоритм
- [5].http://statsoft.ru/home/textbook/modules/stcluan.html#d
- [6].https://tproger.ru/articles/computational-complexity-explained/
- [7].https://habr.com/ru/post/173821/
- [8].https://ru.wikipedia.org/wiki/Временная_сложность_алгоритма
- [9].https://ru.wikipedia.org/wiki/A*#Оценка_сложности
- [10].https://habr.com/ru/post/71083/
- [11].https://towardsdatascience.com/a-star-a-search-algorithm-eb495fb156bb
- [12].https://github.com/kodek16/brestprog/blob/master/topics/dijkstra/dijkstra.md
- [13].https://nuancesprog.ru/p/13513/
- [14].https://dic.academic.ru/dic.nsf/ruwiki/614147

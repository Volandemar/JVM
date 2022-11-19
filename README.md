При запуске написанного кода происходит подгрузка **JvmComprehension** класса с использованием загрузчиков **ClassLoader** (Application, Platform, Bootstrap и пользовательские). **ClassLoader** выполняет проверку кода и определяет необходимость использования сторонних классов (в представленном коде это **Object, Integer** и **System**).

Все подгрузки осуществляются в **Metaspace** - одна из областей памяти выделенной для работы кода. А при загрузке самой программы выделяются две другие области памяти:

- **Stack** **Memory** отвечает за хранение ссылок на объекты кучи и за хранение типов значений, которые содержат само значение, а не ссылку на объект из кучи.

- **Heap** (куча) хранит в памяти фактические объекты, на которые ссылаются переменные из **Stack Memory**.

Далее по строкам кода:

Идет вызов метода **Main**, в результате чего создается фрейм **main** в памяти **Stack Memory**.

1. В фрейме **main** создается переменная **i** со значением **1**
1. В **Heap** создается объект типа **Object** и в фрейм **main** (**Stack Memory**) добавляется имя данного объекта «**о**». Между именем объекта и самим объектов (расположенным в **Heap**) создается связь.
1. В **Heap** создается объект типа **Integer** и в фрейм **main** добавляется переменная **ii** со значением **2**. Между переменной и самим объектов (расположенным в **Heap**) создается связь.
1. Вывод метода **printAll**. В рамках это создается новый фрейм **printAll** в **Stack Memory**, копируются имена переменных **I**, **ii** и **о** из фрейма **main** со всеми ссылками на **Heap.**
1. В **Heap** создается объект типа **Integer** и в фрейм **printAll** добавляется переменная **uselessVar** со значением **700**. Между переменной и самим объектов (расположенным в **Heap**) создается связь.
1. Создается новый фрейм **toString** в памяти **StackMemory**, копируется переменная **о** со ссылкой на **Heap**. Далее создается еще один фрейм **out.println**, со ссылкой на подгруженный в **Heap** объект **System**. После идет выполнение фрейма **toString**, его значение передается в **out.println** и дополнительно копируются значения **i** и **ii**. По окончанию выполнения **toString** выгружается из памяти **StackMemory** и идет реализация фрейма **out.println,** которая выводит на экран значения **o.toString, i** и **ii. Out.println** выгружается из памяти **StackMemory,** объект **System** находящийся в **Heap** так же помечается на выгрузку (будет реализовано при запуске сборщика мусора). Фрейм printAll со всеми копиями переменных и связей так же выгружается из памяти **StackMemory.**
1. Создается новый фрейм **out.println** в память **StackMemory** со ссылкой на объект **System** (2 варианта: 1 – если сборщик мусора не запущен, то на имеющийся в **Heap**; 2 – в противном случае создается новый объект **System**). Передается переменная **String** со значением «**finished**» и выполняется метод **out.println** c выводом на экран значения переменной **String.** Далее идет выгрузка фрейма **out.println** из памяти **StackMemory**, объект **System** помечается на выгрузку. Метод **main** завершает работу. Фрейм **main** со своими переменными, объекты **Integer** и **Object** помечаются на выгрузку. Все очищается, и программа завершается.

# Классы и структуры

В отличии от С++, где классы и структуры это почти одно и тоже,
в D они имею различное назначение, поведение и возможности.

* [Классы](#классы)
* [Структуры](#структуры)

## Классы

Классы предназначены для реализации классической концепции ООП.
В отличии от C++ множественное наследование запрещено, все классы
имеют единого предка -- `class Object`. Классы имеют ссылочную
семантику и всегда создаются в куче.

Пример:

```c++
class SomeClass
{
public: // в D по умолчанию модификаторы доступа и наследования всегда 'public'
    void foo(int x)
    {
        ...
    }
}; // в D не нужно ставить ';' после объявления класса или структуры
```

D:
```d
SomeClass a;
assert (a is null);
a = new SomeClass;
a.foo(12);
```

C++:
```c++
SomeClass* a;
a = new SomeClass();
a->foo(12);
```

Создать класс на стеке можно [средствами стандартной библиотеки](https://dlang.org/phobos/std_typecons.html#scoped).

Все методы класса в D по умолчанию виртуальные, а для переопределения
в наследнике нужно использовать ключевое слово `override`. Не являются
виртуальными только приватные и помеченные как `final` методы.

Отсутствие множественного наследования компенсируется механизмом реализации
интерфейсов. Интерфейс похож на класс с чистыми виртуальными методами
(`virtual void foo(int x) = 0;`), который не имеет полей (данных). Концепция
схожа с интерфейсами в Java, но допускается реализация `final` методов.

```d
import std.stdio;

interface Foo { int func1(int x); }

interface Bar { int func2(int x); }

class Base
{
    void func3() { writeln(__FUNCTION__); }
    void func4(int x) { writeln(x^^3); }
}

class E : Base, Foo, Bar
{
    override // можно группировать атрибуты
    {
        int func1(int x) { return x * 2; }
        int func2(int x) { return x * x; }
    }

    override void func4(int x) { writeln(x^^4); }
}
```

## Структуры

Структуры в D нельзя наследовать. Они передаются по значению и создаются на стеке.
Это простые типы данных, которые не содержат ничего кроме полей (отсутствуют 
виртуальные таблицы, мьютексы и всё остальное что присутствует по умолчанию
в классе). Все методы структур не виртуальны (т.к. запрещено наследование).

WIP
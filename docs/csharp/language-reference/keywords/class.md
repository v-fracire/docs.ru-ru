---
title: Ключевое слово class (справочник по C#)
ms.date: 07/18/2017
f1_keywords:
- class_CSharpKeyword
- class
helpviewer_keywords:
- class keyword [C#]
ms.assetid: b95d8815-de18-4c3f-a8cc-a0a53bdf8690
ms.openlocfilehash: 3f30fb473b486efc8381faa9076b98763935b0ae
ms.sourcegitcommit: 2eb5ca4956231c1a0efd34b6a9cab6153a5438af
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/11/2018
ms.locfileid: "49086068"
---
# <a name="class-c-reference"></a>класс (Справочник по C#)

Классы объявляются с помощью ключевого слова `class`, как показано в следующем примере:

```csharp
class TestClass
{
    // Methods, properties, fields, events, delegates
    // and nested classes go here.
}
```

## <a name="remarks"></a>Примечания

В C# допускается только одиночное наследование. Другими словами, класс может наследовать реализацию только от одного базового класса. Однако класс может реализовывать несколько интерфейсов. В таблице ниже приведены примеры наследования класса и реализации интерфейса.

|Наследование|Пример|
|-----------------|-------------|
|Нет|`class ClassA { }`|
|Single|`class DerivedClass: BaseClass { }`|
|Отсутствует, реализует два интерфейса|`class ImplClass: IFace1, IFace2 { }`|
|Одиночное, реализует один интерфейс|`class ImplDerivedClass: BaseClass, IFace1 { }`|

Классы, объявленные непосредственно в пространстве имен и не вложенные в другие классы, могут быть [открытыми](../../../csharp/language-reference/keywords/public.md) или [внутренними](../../../csharp/language-reference/keywords/internal.md). По умолчанию классы являются `internal`.

Члены класса, включая вложенные классы, могут объявляться с типом доступа [public](public.md), [protected internal](protected-internal.md), [protected](protected.md), [internal](internal.md), [private](private.md) или [private protected](private-protected.md). По умолчанию члены имеют тип доступа `private`.

Дополнительные сведения см. в статье [Модификаторы доступа](../../../csharp/programming-guide/classes-and-structs/access-modifiers.md).

Можно объявить универсальные классы, имеющие параметры типа. Дополнительные сведения см. в разделе [Универсальные классы](../../../csharp/programming-guide/generics/generic-classes.md).

Класс может содержать объявления следующих членов:

- [Конструкторы](../../../csharp/programming-guide/classes-and-structs/constructors.md)

- [Константы](../../../csharp/programming-guide/classes-and-structs/constants.md)

- [Поля](../../../csharp/programming-guide/classes-and-structs/fields.md)

- [Методы завершения](../../../csharp/programming-guide/classes-and-structs/destructors.md)

- [Методы](../../../csharp/programming-guide/classes-and-structs/methods.md)

- [Свойства](../../../csharp/programming-guide/classes-and-structs/properties.md)

- [Индексаторы](../../../csharp/programming-guide/indexers/index.md)

- [Операторы](../../../csharp/programming-guide/statements-expressions-operators/operators.md)

- [События](../../../csharp/programming-guide/events/index.md)

- [Делегаты](../../../csharp/programming-guide/delegates/index.md)

- [Классы](../../../csharp/programming-guide/classes-and-structs/classes.md)

- [Интерфейсы](../../../csharp/programming-guide/interfaces/index.md)

- [Структуры](../../../csharp/programming-guide/classes-and-structs/structs.md)

- [Перечисления](../../../csharp/programming-guide/enumeration-types.md)

## <a name="example"></a>Пример

В приведенном ниже примере показано объявление полей, конструкторов и методов класса. В нем также демонстрируется создание экземпляра объекта и печать данных экземпляра. В этом примере объявляются два класса. Первый класс, `Child`, содержит два частных поля (`name` и `age`), два общих конструктора и один общий метод. Второй класс, `StringTest`, используется для хранения `Main`.

[!code-csharp[csrefKeywordsTypes#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/keywordsTypes.cs#5)]

## <a name="comments"></a>Комментарии

Обратите внимание, что в предыдущем примере доступ к частным полям (`name` и `age`) возможен только с помощью общих методов класса `Child`. Например, имя ребенка нельзя напечатать из метода `Main` с помощью следующего оператора:

```csharp
Console.Write(child1.name);   // Error
```

Получить доступ к закрытым членам класса `Child` из метода `Main` можно было бы лишь в том случае, если бы `Main` был членом класса.

Типы, объявленные в классе без модификатора доступа, по умолчанию являются `private`, поэтому члены данных в этом примере останутся `private`, если ключевые слова будут удалены.

Наконец, обратите внимание, что для объекта, созданного с помощью конструктора по умолчанию (`child3`), поле age по умолчанию было инициализировано с нулевым значением.

## <a name="c-language-specification"></a>Спецификация языка C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>См. также

- [Справочник по C#](../../../csharp/language-reference/index.md)  
- [Руководство по программированию на C#](../../../csharp/programming-guide/index.md)  
- [Ключевые слова в C#](../../../csharp/language-reference/keywords/index.md)  
- [Ссылочные типы](../../../csharp/language-reference/keywords/reference-types.md)

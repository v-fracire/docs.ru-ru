---
title: Оператор using (Справочник по C#)
ms.date: 07/20/2015
helpviewer_keywords:
- using statement [C#]
ms.assetid: afc355e6-f0b9-4240-94dd-0d93f17d9fc3
ms.openlocfilehash: ec4849dda0f28ad1f0e0ebb2c493a33005107db4
ms.sourcegitcommit: 2eceb05f1a5bb261291a1f6a91c5153727ac1c19
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2018
ms.locfileid: "43500976"
---
# <a name="using-statement-c-reference"></a>Оператор using (Справочник по C#)
Предоставляет удобный синтаксис, обеспечивающий правильное использование объектов <xref:System.IDisposable>.  
  
## <a name="example"></a>Пример  
 В следующем примере показано использование оператора `using`.  
  
 [!code-csharp[csrefKeywordsNamespace#4](../../../csharp/language-reference/keywords/codesnippet/CSharp/using-statement_1.cs)]  
  
## <a name="remarks"></a>Примечания  
 <xref:System.IO.File> и <xref:System.Drawing.Font> представляют собой примеры управляемых типов, которые обращаются к неуправляемым ресурсам (в данном случае это обработчики файлов и контексты устройств). Существуют и многие другие виды неуправляемых ресурсов и типов библиотек классов, которые их инкапсулируют. Все эти типы реализуют интерфейс <xref:System.IDisposable>.  
  
Когда время существования объекта `IDisposable` ограничено одним методом, необходимо объявить его и создать его экземпляр в операторе `using`. Оператор `using` правильно вызывает метод <xref:System.IDisposable.Dispose%2A> для объектов, а также (если он используется, как показано выше) становится причиной выхода объекта из области действия, как только вызывается метод <xref:System.IDisposable.Dispose%2A>. В блоке `using` объект доступен только для чтения, и изменить или переназначить его нельзя.  
  
 Использование оператора `using` обеспечивает вызов метода <xref:System.IDisposable.Dispose%2A>, даже если в блоке `using` возникает исключение. Тот же результат можно получить, поместив объект в блок `try`, а затем вызвав метод <xref:System.IDisposable.Dispose%2A> в блоке `finally`; фактически компилятор переводит оператор `using` именно так. Во время компиляции представленный выше код разворачивается в следующий (обратите внимание на дополнительные фигурные скобки, создающие ограниченную область действия для объекта):  
  
 [!code-csharp[csrefKeywordsNamespace#5](../../../csharp/language-reference/keywords/codesnippet/CSharp/using-statement_2.cs)]  
 
 Дополнительные сведения об операторе `try`-`finally` см. в разделе [try-finally](try-finally.md).
  
 В операторе `using` можно объявить сразу несколько экземпляров типа, как показано в следующем примере:  
  
 [!code-csharp[csrefKeywordsNamespace#6](../../../csharp/language-reference/keywords/codesnippet/CSharp/using-statement_3.cs)]  
  
 Вы можете создать экземпляр объекта ресурсов, а затем передать переменную в оператор `using`, однако делать этого не рекомендуется. В этом случае объект остается в области действия и после того, как блок `using` теряет контроль, хотя доступа к неуправляемым ресурсам у него, скорее всего, не будет. Иными словами, он уже не полностью инициализируется. При попытке использовать объект за пределами блока `using` может возникнуть исключение. По этой причине лучше создать экземпляр объекта в операторе `using` и ограничить область действия блоком `using`.  
  
 [!code-csharp[csrefKeywordsNamespace#7](../../../csharp/language-reference/keywords/codesnippet/CSharp/using-statement_4.cs)]  
  
Дополнительные сведения об утилизации объектов `IDisposable` см. в разделе [Использование объектов, реализующих IDisposable](../../../standard/garbage-collection/using-objects.md).

## <a name="c-language-specification"></a>Спецификация языка C#  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>См. также

- [Справочник по C#](../../../csharp/language-reference/index.md)  
- [Руководство по программированию на C#](../../../csharp/programming-guide/index.md)  
- [Ключевые слова в C#](../../../csharp/language-reference/keywords/index.md)  
- [Директива using](../../../csharp/language-reference/keywords/using-directive.md)  
- [Сборка мусора](../../../standard/garbage-collection/index.md)  
- [Использование объектов, реализующих IDisposable](../../../standard/garbage-collection/using-objects.md)  
- [Интерфейс IDisposable](xref:System.IDisposable)

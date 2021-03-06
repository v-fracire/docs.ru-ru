---
title: Недопустимый режим файла
ms.date: 07/20/2015
f1_keywords:
- vbrID54
ms.assetid: 74891e96-884b-4c8d-872d-cd11ae272372
ms.openlocfilehash: bccbbbeb79f38790a4664b0152ca3378fb55448d
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
ms.locfileid: "33587388"
---
# <a name="bad-file-mode"></a>Недопустимый режим файла
Операторы, используемые для управления содержимым файла должны соответствовать режим, в котором открыт файл. Возможные причины:  
  
-   Объект `FilePutObject` или `FileGetObject` инструкция указывает последовательный файл.  
  
-   Объект `Print` инструкция указывает файл, открытый в режиме доступа, отличный от `Output` или `Append`.  
  
-   `Input` Инструкция указывает файл, открытый в режиме доступа, отличный от `Input`  
  
-   Попытка записи в файл только для чтения.  
  
## <a name="to-correct-this-error"></a>Исправление ошибки  
  
-   Убедитесь, что `FilePutObject` и `FileGetObject` ссылаются только на файлы, открытые для `Random` или `Binary` доступа.  
  
-   Убедитесь, что `Print` указывает файл, открытый в `Output` или `Append` режим доступа. В противном случае используйте другой инструкции для размещения данных в файл или открыть файл в нужный режим.  
  
-   Убедитесь, что `Input` задает файл, открытый для `Input`. В противном случае используйте другой инструкции для размещения данных в файл или открыть файл в нужный режим.  
  
-   При записи в файл только для чтения, изменения состояния чтения и записи файла или не пытайтесь выполнить запись в него.  
  
-   Используйте функциональность объекта `My.Computer.FileSystem` .  
  
## <a name="see-also"></a>См. также  
 <xref:Microsoft.VisualBasic.FileSystem>  
 [Исправление неполадок, связанных с чтением из текстовых файлов и записью в такие файлы](../../../visual-basic/developing-apps/programming/drives-directories-files/troubleshooting-reading-from-and-writing-to-text-files.md)

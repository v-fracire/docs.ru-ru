---
title: Практическое руководство. Предоставление диалогового окна "Ход выполнения" для операций с файлами (Руководство по программированию на C#)
ms.date: 07/20/2015
helpviewer_keywords:
- progress dialog [C#]
ms.assetid: 01b71fe7-8178-4dc8-aeb1-12053be7b51c
ms.openlocfilehash: e93ce06a01046dfaf4465470ba7fdc687effa58d
ms.sourcegitcommit: fb78d8abbdb87144a3872cf154930157090dd933
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2018
ms.locfileid: "47207407"
---
# <a name="how-to-provide-a-progress-dialog-box-for-file-operations-c-programming-guide"></a>Практическое руководство. Предоставление диалогового окна "Ход выполнения" для операций с файлами (Руководство по программированию на C#)
Вы можете предоставить стандартное диалоговое окно, в котором будет отображаться ход выполнения операций с файлами в Windows, при использовании метода <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyFile%28System.String%2CSystem.String%2CMicrosoft.VisualBasic.FileIO.UIOption%29> из пространства имен <xref:Microsoft.VisualBasic?displayProperty=nameWithType>.  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-add-a-reference-in-visual-studio"></a>Добавление ссылки в Visual Studio  
  
1.  В строке меню выберите **Проект**, **Добавить ссылку**.  
  
     Откроется диалоговое окно **Диспетчер ссылок**.  
  
2.  В области **Сборки** выберите **Платформа**, если этот вариант еще не выбран.  
  
3.  В списке имен установите флажок **Microsoft.VisualBasic**, а затем нажмите кнопку **ОК**, чтобы закрыть диалоговое окно.  
  
## <a name="example"></a>Пример  
 Следующий код копирует каталог, указанный `sourcePath`, в каталог, указанный `destinationPath`. Этот код также предоставляет стандартное диалоговое окно, в котором отображается примерное время, оставшееся до завершения операции.  
  
 [!code-csharp[csFilesandFolders#11](../../../csharp/programming-guide/file-system/codesnippet/CSharp/how-to-provide-a-progress-dialog-box-for-file-operations_1.cs)]  
  
## <a name="see-also"></a>См. также

- [Файловая система и реестр (руководство по программированию на C#)](../../../csharp/programming-guide/file-system/index.md)

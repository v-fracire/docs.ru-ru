---
title: Маршалинг с помощью COM- взаимодействия
ms.date: 09/07/2017
helpviewer_keywords:
- COM interop, data marshaling
- marshaling data, COM interop
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: d0d94223223568efe921af3a340815a966cc6c6f
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
ms.locfileid: "33388359"
---
# <a name="marshaling-data-with-com-interop"></a>Маршалинг с помощью COM- взаимодействия
COM-взаимодействие обеспечивает поддержку как для использования COM-объектов из управляемого кода, так и для предоставления доступа к управляемым объектам для COM. Поддержка маршалинга данных в COM и обратно достаточно полная и почти всегда обеспечивает правильное поведение маршалинга.  
  
 [!INCLUDE[winsdklong](../../../includes/winsdklong-md.md)] включает следующие средства COM-взаимодействия:  
  
-   [Средство импорта библиотек типов (Tlbimp.exe)](../../../docs/framework/tools/tlbimp-exe-type-library-importer.md), которое преобразует библиотеку типов COM в сборку взаимодействия. Эта сборка используется службой маршалинга взаимодействия для создания оболочек, маршалирующих данные между управляемой и неуправляемой памятью.  
  
-   [Средство экспорта библиотек типов (Tlbexp.exe)](../../../docs/framework/tools/tlbexp-exe-type-library-exporter.md), которое создает библиотеку типов COM из сборки и формирует оболочку, которая выполняет маршалинг при вызовах методов.  
  
 Следующие разделы посвящены процессам настройки оболочек взаимодействий для случаев, когда можно (или необходимо) предоставить упаковщику дополнительную информацию о типах.  
  
## <a name="in-this-section"></a>В этом разделе  
[How to: Create Wrappers Manually](how-to-create-wrappers-manually.md)  (Практическое руководство. Создание оболочек вручную)  
Сведения о том, как вручную создать программу-оболочку COM в управляемом исходном коде. 
 
 [Практическое руководство. Миграция DCOM с управляемым кодом в WCF](../../../docs/framework/interop/how-to-migrate-managed-code-dcom-to-wcf.md)  
 Сведения о переносе управляемого кода DCOM в WCF для получения наиболее безопасного решения.  
  
## <a name="related-sections"></a>Связанные разделы  
 [Типы данных COM](https://msdn.microsoft.com/library/sak564ww(v=vs.100).aspx)  
 Содержит описание соответствующих управляемых и неуправляемых типов данных.  
  
 [Настройка вызываемых оболочек COM](https://msdn.microsoft.com/library/3bwc828w(v=vs.100).aspx)  
 В этой статье описан способ явного маршалинга типов данных с использованием атрибута <xref:System.Runtime.InteropServices.MarshalAsAttribute> во время разработки.  
  
 [Настройка вызываемых оболочек времени выполнения](https://msdn.microsoft.com/library/e753eftz(v=vs.100).aspx)  
 Описаны способы настройки связанного с маршалингом поведения типов в сборке взаимодействия и определения типов COM вручную.  
  
 [Расширенное COM-взаимодействие](https://msdn.microsoft.com/library/bd9cdfyx(v=vs.100).aspx)  
 Приводятся ссылки на дополнительные сведения о включении COM-компонентов в разрабатываемое приложение .NET Framework.  
  
 [Общие сведения о преобразовании сборки в библиотеку типов](https://msdn.microsoft.com/library/xk1120c3(v=vs.100).aspx)  
 Описывается процесс преобразования при экспорте сборки в библиотеку типов.  
  
 [Общие сведения о преобразовании библиотеки типов в сборку](https://msdn.microsoft.com/library/k83zzh38(v=vs.100).aspx)  
 Описывается процесс преобразования при экспорте библиотеки типов в сборку.  
  
 [Взаимодействие с помощью универсальных типов](https://msdn.microsoft.com/library/ms229590(v=vs.100).aspx)  
 Описываются действия, поддерживаемые при использовании универсальных типов для взаимодействия COM.

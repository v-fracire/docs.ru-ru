---
title: Перенос приложений удаленного взаимодействия .NET на платформу WCF
ms.date: 03/30/2017
helpviewer_keywords:
- ',NET remoting [WCF]'
ms.assetid: 24793465-65ae-4308-8c12-dce4fd12a583
ms.openlocfilehash: 3f5556eeaf56ac48dd4f1d578ed2e66b90415677
ms.sourcegitcommit: 2eceb05f1a5bb261291a1f6a91c5153727ac1c19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2018
ms.locfileid: "43503438"
---
# <a name="migrating-net-remoting-applications-to-wcf"></a>Перенос приложений удаленного взаимодействия .NET на платформу WCF
**Этот раздел относится к технологии прежних версий, которая сохраняется для обеспечения обратной совместимости с существующими приложениями и не рекомендуется для разработки новых приложений. Теперь распределенные приложения следует разрабатывать с использованием WCF.**  
  
 Существует два способа, чтобы воспользоваться преимуществами WCF с существующими приложениями удаленного взаимодействия .NET: интеграция и миграция. Благодаря интеграции вы можете иметь .net Remoting 2.0 и WCF под управлением параллельно, позволяя одновременно предоставлять же бизнес-объектов посредством обеих технологий без необходимости изменения существующих .net Remoting 2.0 кода. Интеграция требует использования [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)] 2.0 или выше. Если вы хотите воспользоваться преимуществами функций WCF и не обязательно передачи совместимости с системами Remoting 2.0, можно перенести всей службы WCF. Миграция из .NET Remoting 2.0 на платформу WCF требует внесения изменений в интерфейс удаленного объекта и его параметры конфигурации. В этих разделах рассматриваются [из удаленного взаимодействия на платформу Windows Communication Foundation](https://go.microsoft.com/fwlink/?LinkId=74403).  
  
## <a name="see-also"></a>См. также  
 [Концептуальный обзор](../../../../docs/framework/wcf/conceptual-overview.md)

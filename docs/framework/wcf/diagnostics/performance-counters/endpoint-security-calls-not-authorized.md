---
title: 'Конечная точка: количество неавторизованных вызовов системы безопасности'
ms.date: 03/30/2017
ms.assetid: d25095ff-9ff0-4c69-a674-4e6a9fe3f4dc
ms.openlocfilehash: b37d4cd33c41c6e978dd82ca7ce6332302a843de
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/28/2018
ms.locfileid: "50198033"
---
# <a name="endpoint-security-calls-not-authorized"></a>Конечная точка: количество неавторизованных вызовов системы безопасности
Имя счетчика: Security Calls Not Authorized.  
  
## <a name="description"></a>Описание  
 Значение этого счетчика увеличивается, когда метод <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccess%2A> возвращает значение `false`. Это показывает, что входящее сообщение было надлежащим образом защищено и поступило от допустимого пользователя, не авторизованного для выполнения определенных задач.

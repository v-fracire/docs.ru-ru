---
title: 'Служба: количество сбоев при проверке безопасности и проверке подлинности'
ms.date: 03/30/2017
ms.assetid: 55c98268-b1ad-459d-851b-25ef52248187
ms.openlocfilehash: ba8da3ae6be6bd089690359f19e153da1e0b54fc
ms.sourcegitcommit: 5fd80619c760fa8c25d33a6f5661247cb65da465
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2018
ms.locfileid: "50744253"
---
# <a name="service-security-validation-and-authentication-failures"></a>Служба: количество сбоев при проверке безопасности и проверке подлинности
Имя счетчика: Security Validation and Authentication Failures  
  
## <a name="description"></a>Описание  
 Значение этого счетчика увеличивается всякий раз, когда сообщение отклоняется из-за проблемы безопасности, не относящейся к счетчику "Security Calls Not Authorized". К таким проблемам относятся следующие.  
  
-   Невозможно прочесть в этом сообщении маркер клиента.  
  
-   Маркер клиента не прошел проверку подлинности (например, неправильный пароль).  
  
-   Сбой при проверке подписи (например, сообщение было искажено).  
  
-   Сообщение является копией предыдущего, что может свидетельствовать об атаке воспроизведения.  
  
-   Произошел сбой при расшифровке.  
  
-   В сообщении отсутствуют некоторые обязательные элементы (например, отсутствует метка времени или зашифрованный блок данных).  
  
-   Во время подтверждения TLSNEGO/SPNEGO произошли ошибки.

---
title: Структура MDAInfo
ms.date: 03/30/2017
api_name:
- MDAInfo
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- MDAInfo
helpviewer_keywords:
- MDAInfo structure [.NET Framework hosting]
ms.assetid: fb8c14f7-d461-43d1-8b47-adb6723b9b93
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 5164e85ecc97de99dcc493c2ba5efa8fc3468471
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
ms.locfileid: "33443184"
---
# <a name="mdainfo-structure"></a>Структура MDAInfo
Предоставляет подробные сведения о `Event_MDAFired` событие, которое инициирует создание управляемого помощника по отладке (MDA).  
  
## <a name="syntax"></a>Синтаксис  
  
```  
typedef struct _MDAInfo {  
    LPCWSTR  lpMDACaption;  
    LPCWSTR  lpMDAMessage  
} MDAInfo;  
```  
  
## <a name="members"></a>Участники  
  
|Член|Описание|  
|------------|-----------------|  
|`lpMDACaption`|Название текущего MDA. Заголовок описывает тип ошибки, вызвавшей `Event_MDAFired` событий.|  
|`lpMDAMessage`|Выходное сообщение, предоставленные текущим MDA.|  
  
## <a name="remarks"></a>Примечания  
 Создание дампов дополнительной информации о состоянии или средств отладки, в сочетании с общеязыковой среды выполнения (CLR) для выполнения задач, таких как идентификация недопустимых состояний в ядре выполнения среды выполнения управляемых помощников по отладке (MDA) модуль. Помощники отладки управляемого кода создавать XML-сообщения о событиях, которые в противном случае сложно для перехвата исключения. Они особенно полезны для отладки переходов между управляемым и неуправляемым кодом.  
  
 При возникновении события, которое вызывает создание MDA, среда выполнения выполняет следующие действия:  
  
-   Если узел не зарегистрировал [IActionOnCLREvent](../../../../docs/framework/unmanaged-api/hosting/iactiononclrevent-interface.md) экземпляр путем вызова [ICLROnEventManager::RegisterActionOnEvent](../../../../docs/framework/unmanaged-api/hosting/iclroneventmanager-registeractiononevent-method.md) получать уведомления об `Event_MDAFired` события, среда выполнения выполняет его по умолчанию поведение без размещения.  
  
-   Если узел зарегистрировал обработчик этого события, среда выполнения проверяет, присоединен ли отладчик к процессу. Если это так, среда выполнения переключается в режим отладчика. Когда отладчик продолжает выполнение, она вызывает в узле. Если отладчик не присоединен, среда выполнения вызывает `IActionOnCLREvent::OnEvent` и передает указатель на `MDAInfo` экземпляр как `data` параметр.  
  
 Узел можно активировать MDA и получать уведомления при активации MDA. Это дает возможность для переопределения поведения по умолчанию и прекращения управляемого потока, вызвавшего событие, для предотвращения повреждения состояния процесса узла. Дополнительные сведения об использовании MDA см. в разделе [Диагностика ошибок посредством управляемых помощников по отладке](../../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md).  
  
## <a name="requirements"></a>Требования  
 **Платформы:** разделе [требования к системе для](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** MSCorEE.idl  
  
 **Библиотека:** включена как ресурс в MSCorEE.dll  
  
 **Версии платформы .NET framework:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]  
  
## <a name="see-also"></a>См. также  
 [Структуры размещения](../../../../docs/framework/unmanaged-api/hosting/hosting-structures.md)  
 [Диагностика ошибок посредством помощников по отладке управляемого кода](../../../../docs/framework/debug-trace-profile/diagnosing-errors-with-managed-debugging-assistants.md)

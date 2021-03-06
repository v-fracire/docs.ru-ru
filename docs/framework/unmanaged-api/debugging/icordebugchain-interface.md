---
title: ICorDebugChain интерфейс1
ms.date: 03/30/2017
api_name:
- ICorDebugChain
api_location:
- mscordbi.dll
api_type:
- COM
f1_keywords:
- ICorDebugChain
helpviewer_keywords:
- ICorDebugChain interface [.NET Framework debugging]
ms.assetid: f671f519-1cb3-4ae5-b9f1-abc5e783459f
topic_type:
- apiref
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: 32889a8e8867fc42b48413463095dda423f26b85
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
---
# <a name="icordebugchain-interface1"></a>ICorDebugChain интерфейс1
Представляет сегмент физического или логического стека вызовов.  
  
## <a name="methods"></a>Методы  
  
|Метод|Описание|  
|------------|-----------------|  
|[Метод EnumerateFrames](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-enumerateframes-method.md)|Возвращает перечислитель, содержащий все управляемые фреймы стека в цепочке, начиная с последнего кадра.|  
|[Метод GetActiveFrame](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-getactiveframe-method.md)|Получает активный (то есть последней) кадра в цепочке.|  
|[Метод GetCallee](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-getcallee-method.md)|Получает цепь, вызванной этой цепи.|  
|[Метод GetCaller](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-getcaller-method.md)|Получает цепь, вызвавшую данную цепь.|  
|[Метод GetContext](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-getcontext-method.md)|Не реализовано.|  
|[Метод GetNext](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-getnext-method.md)|Получает следующую цепь кадров для потока.|  
|[Метод GetPrevious](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-getprevious-method.md)|Возвращает предыдущую цепь кадров для потока.|  
|[Метод GetReason](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-getreason-method.md)|Возвращает причину происхождения данной вызывающей цепи.|  
|[Метод GetRegisterSet](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-getregisterset-method.md)|Получает набор регистров для активной части этой цепи.|  
|[Метод GetStackRange](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-getstackrange-method.md)|Возвращает диапазон адресов сегмента стека для этой цепи.|  
|[Метод GetThread](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-getthread-method.md)|Получает часть физическом потоке, — это цепочка вызовов.|  
|[Метод IsManaged](../../../../docs/framework/unmanaged-api/debugging/icordebugchain-ismanaged-method.md)|Возвращает значение, указывающее, выполняется ли эта цепочка управляемого кода.|  
  
## <a name="remarks"></a>Примечания  
 Кадры стека в цепи занимают непрерывное пространство стека и используют один поток и контекст. Цепь может представлять любой цепочки управляемого или неуправляемого кода. Пустой `ICorDebugChain` экземпляр представляет неуправляемую цепь.  
  
> [!NOTE]
>  Этот интерфейс не поддерживает удаленные вызовы между компьютерами или между процессами.  
  
## <a name="requirements"></a>Требования  
 **Платформы:** разделе [требования к системе для](../../../../docs/framework/get-started/system-requirements.md).  
  
 **Заголовок:** CorDebug.idl, CorDebug.h  
  
 **Библиотека:** CorGuids.lib  
  
 **Версии платформы .NET framework:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>См. также  
 [Интерфейсы отладки](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)

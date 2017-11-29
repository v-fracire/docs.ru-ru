---
title: "Метод ICorProfilerCallback::RuntimeResumeFinished"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ICorProfilerCallback.RuntimeResumeFinished
api_location: mscorwks.dll
api_type: COM
f1_keywords: ICorProfilerCallback::RuntimeResumeFinished
helpviewer_keywords:
- RuntimeResumeFinished method [.NET Framework profiling]
- ICorProfilerCallback::RuntimeResumeFinished method [.NET Framework profiling]
ms.assetid: 76de0494-dc49-426b-887d-bee98806a982
topic_type: apiref
caps.latest.revision: "11"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: 5d923db2d9614a4cf7ebc4f1d910a86fee85a0bb
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2017
---
# <a name="icorprofilercallbackruntimeresumefinished-method"></a><span data-ttu-id="fdfa5-102">Метод ICorProfilerCallback::RuntimeResumeFinished</span><span class="sxs-lookup"><span data-stu-id="fdfa5-102">ICorProfilerCallback::RuntimeResumeFinished Method</span></span>
<span data-ttu-id="fdfa5-103">Уведомляет профилировщик, что среда выполнения возобновил все потоки среды выполнения и вернулся к нормальной работе.</span><span class="sxs-lookup"><span data-stu-id="fdfa5-103">Notifies the profiler that the runtime has resumed all runtime threads and has returned to normal operation.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="fdfa5-104">Синтаксис</span><span class="sxs-lookup"><span data-stu-id="fdfa5-104">Syntax</span></span>  
  
```  
HRESULT RuntimeResumeFinished();  
```  
  
## <a name="remarks"></a><span data-ttu-id="fdfa5-105">Примечания</span><span class="sxs-lookup"><span data-stu-id="fdfa5-105">Remarks</span></span>  
 <span data-ttu-id="fdfa5-106">`RuntimeResumeFinished` Обратного вызова, не обязательно находиться на том же потоке [ICorProfilerCallback::RuntimeSuspendStarted](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-runtimesuspendstarted-method.md) обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="fdfa5-106">The `RuntimeResumeFinished` callback is not guaranteed to occur on the same thread as the [ICorProfilerCallback::RuntimeSuspendStarted](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-runtimesuspendstarted-method.md) callback.</span></span> <span data-ttu-id="fdfa5-107">Тем не менее, оно возникает в том же потоке, как [ICorProfilerCallback::RuntimeResumeStarted](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-runtimeresumestarted-method.md) обратного вызова.</span><span class="sxs-lookup"><span data-stu-id="fdfa5-107">However, it is guaranteed to occur on the same thread as the [ICorProfilerCallback::RuntimeResumeStarted](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-runtimeresumestarted-method.md) callback.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="fdfa5-108">Требования</span><span class="sxs-lookup"><span data-stu-id="fdfa5-108">Requirements</span></span>  
 <span data-ttu-id="fdfa5-109">**Платформы:** разделе [требования к системе для](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="fdfa5-109">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="fdfa5-110">**Заголовок:** CorProf.idl, CorProf.h</span><span class="sxs-lookup"><span data-stu-id="fdfa5-110">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="fdfa5-111">**Библиотека:** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="fdfa5-111">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="fdfa5-112">**Версии платформы .NET framework:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="fdfa5-112">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="fdfa5-113">См. также</span><span class="sxs-lookup"><span data-stu-id="fdfa5-113">See Also</span></span>  
 [<span data-ttu-id="fdfa5-114">Интерфейс ICorProfilerCallback</span><span class="sxs-lookup"><span data-stu-id="fdfa5-114">ICorProfilerCallback Interface</span></span>](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-interface.md)
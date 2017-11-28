---
title: "Метод ICorProfilerCallback::UnmanagedToManagedTransition"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ICorProfilerCallback.UnmanagedToManagedTransition
api_location: mscorwks.dll
api_type: COM
f1_keywords: ICorProfilerCallback::UnmanagedToManagedTransition
helpviewer_keywords:
- ICorProfilerCallback::UnmanagedToManagedTransition method [.NET Framework profiling]
- UnmanagedToManagedTransition method [.NET Framework profiling]
ms.assetid: ade2cc01-9b81-4e09-a5f9-b3b9dda27e96
topic_type: apiref
caps.latest.revision: "12"
author: mairaw
ms.author: mairaw
manager: wpickett
ms.openlocfilehash: 45a2ceb53263e317c5c72695d6bc1e93f8f70bbb
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2017
---
# <a name="icorprofilercallbackunmanagedtomanagedtransition-method"></a><span data-ttu-id="6bfdc-102">Метод ICorProfilerCallback::UnmanagedToManagedTransition</span><span class="sxs-lookup"><span data-stu-id="6bfdc-102">ICorProfilerCallback::UnmanagedToManagedTransition Method</span></span>
<span data-ttu-id="6bfdc-103">Уведомляет профилировщик о переход от неуправляемого кода в управляемый код.</span><span class="sxs-lookup"><span data-stu-id="6bfdc-103">Notifies the profiler that a transition from unmanaged code to managed code has occurred.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="6bfdc-104">Синтаксис</span><span class="sxs-lookup"><span data-stu-id="6bfdc-104">Syntax</span></span>  
  
```  
HRESULT UnmanagedToManagedTransition(  
    [in] FunctionID functionId,  
    [in] COR_PRF_TRANSITION_REASON reason);  
```  
  
#### <a name="parameters"></a><span data-ttu-id="6bfdc-105">Параметры</span><span class="sxs-lookup"><span data-stu-id="6bfdc-105">Parameters</span></span>  
 `functionId`  
 <span data-ttu-id="6bfdc-106">[in] Идентификатор функции, которая вызывается.</span><span class="sxs-lookup"><span data-stu-id="6bfdc-106">[in] The ID of the function that is being called.</span></span>  
  
 `reason`  
 <span data-ttu-id="6bfdc-107">[in] Значение [COR_PRF_TRANSITION_REASON](../../../../docs/framework/unmanaged-api/profiling/cor-prf-transition-reason-enumeration.md) перечисления, которое указывает, произошло из-за вызова управляемого кода из неуправляемого кода или из-за возврат из неуправляемой функции, вызываемой управляемыми перехода.</span><span class="sxs-lookup"><span data-stu-id="6bfdc-107">[in] A value of the [COR_PRF_TRANSITION_REASON](../../../../docs/framework/unmanaged-api/profiling/cor-prf-transition-reason-enumeration.md) enumeration that indicates whether the transition occurred because of a call into managed code from unmanaged code, or because of a return from an unmanaged function called by a managed one.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="6bfdc-108">Примечания</span><span class="sxs-lookup"><span data-stu-id="6bfdc-108">Remarks</span></span>  
 <span data-ttu-id="6bfdc-109">Если значение `reason` — COR_PRF_TRANSITION_RETURN и `functionId` не равно NULL, функция ID будет соответствовать неуправляемой функции и никогда не будет выполняться компиляция с помощью время JIT-компилятора.</span><span class="sxs-lookup"><span data-stu-id="6bfdc-109">If the value of `reason` is COR_PRF_TRANSITION_RETURN and `functionId` is not null, the function ID is that of the unmanaged function, and will never have been compiled using the just-in-time (JIT) compiler.</span></span> <span data-ttu-id="6bfdc-110">Неуправляемые функции имеют некоторые основные сведения, связанные с ними, такие как имя и некоторые метаданные.</span><span class="sxs-lookup"><span data-stu-id="6bfdc-110">Unmanaged functions have some basic information associated with them, such as a name and some metadata.</span></span>  
  
 <span data-ttu-id="6bfdc-111">Если значение `reason` является COR_PRF_TRANSITION_CALL, возможно, вызываемой функции (то есть, управляемую функцию) еще не JIT-компиляции.</span><span class="sxs-lookup"><span data-stu-id="6bfdc-111">If the value of `reason` is COR_PRF_TRANSITION_CALL, it may be possible that the called function (that is, the managed function) has not yet been JIT-compiled.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="6bfdc-112">Требования</span><span class="sxs-lookup"><span data-stu-id="6bfdc-112">Requirements</span></span>  
 <span data-ttu-id="6bfdc-113">**Платформы:** разделе [требования к системе для](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="6bfdc-113">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="6bfdc-114">**Заголовок:** CorProf.idl, CorProf.h</span><span class="sxs-lookup"><span data-stu-id="6bfdc-114">**Header:** CorProf.idl, CorProf.h</span></span>  
  
 <span data-ttu-id="6bfdc-115">**Библиотека:** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="6bfdc-115">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="6bfdc-116">**Версии платформы .NET framework:**[!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="6bfdc-116">**.NET Framework Versions:** [!INCLUDE[net_current_v20plus](../../../../includes/net-current-v20plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="6bfdc-117">См. также</span><span class="sxs-lookup"><span data-stu-id="6bfdc-117">See Also</span></span>  
 [<span data-ttu-id="6bfdc-118">Интерфейс ICorProfilerCallback</span><span class="sxs-lookup"><span data-stu-id="6bfdc-118">ICorProfilerCallback Interface</span></span>](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-interface.md)  
 [<span data-ttu-id="6bfdc-119">Метод ManagedToUnmanagedTransition</span><span class="sxs-lookup"><span data-stu-id="6bfdc-119">ManagedToUnmanagedTransition Method</span></span>](../../../../docs/framework/unmanaged-api/profiling/icorprofilercallback-managedtounmanagedtransition-method.md)  
 [<span data-ttu-id="6bfdc-120">Использование явного вызова Pinvoke в C++ (атрибут DllImport)</span><span class="sxs-lookup"><span data-stu-id="6bfdc-120">Using Explicit PInvoke in C++ (DllImport Attribute)</span></span>](/cpp/dotnet/using-explicit-pinvoke-in-cpp-dllimport-attribute)  
 [<span data-ttu-id="6bfdc-121">Использование взаимодействия языка C++ (неявный PInvoke)</span><span class="sxs-lookup"><span data-stu-id="6bfdc-121">Using C++ Interop (Implicit PInvoke)</span></span>](/cpp/dotnet/using-cpp-interop-implicit-pinvoke)
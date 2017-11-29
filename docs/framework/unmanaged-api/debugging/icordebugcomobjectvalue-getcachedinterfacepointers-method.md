---
title: "Метод ICorDebugComObjectValue::GetCachedInterfacePointers"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ICorDebugComObjectValue::GetCachedInterfacePointers
api_location: mscordbi.dll
f1_keywords: ICorDebugComObjectValue::GetCachedInterfacePointers
helpviewer_keywords:
- ICorDebugComObjectValue::GetCachedInterfacePointers method [.NET Framework debugging]
- GetCachedInterfacePointers method, ICorDebugComObjectValue interface [.NET Framework debugging]
ms.assetid: 08dbd558-bd39-4263-94c2-71e70687aaf0
topic_type: apiref
caps.latest.revision: "7"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: 607300d6b46f3ee20545c50872b99df483b1614c
ms.sourcegitcommit: 4f3fef493080a43e70e951223894768d36ce430a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/21/2017
---
# <a name="icordebugcomobjectvaluegetcachedinterfacepointers-method"></a><span data-ttu-id="a3158-102">Метод ICorDebugComObjectValue::GetCachedInterfacePointers</span><span class="sxs-lookup"><span data-stu-id="a3158-102">ICorDebugComObjectValue::GetCachedInterfacePointers Method</span></span>
<span data-ttu-id="a3158-103">Возвращает интерфейс необработанные указатели, кэшируются на текущем вызываемой оболочки времени выполнения (RCW).</span><span class="sxs-lookup"><span data-stu-id="a3158-103">Gets the raw interface pointers cached on the current runtime callable wrapper (RCW).</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="a3158-104">Синтаксис</span><span class="sxs-lookup"><span data-stu-id="a3158-104">Syntax</span></span>  
  
```  
HRESULT GetCachedInterfacePointers(  
    [in] BOOL bIInspectableOnly,  
    [in] ULONG32 celt,  
    [out] ULONG32 *pceltFetched,  
    [out, size_is(celt), length_is(*pceltFetched) CORDB_ADDRESS *ptrs);  
```  
  
#### <a name="parameters"></a><span data-ttu-id="a3158-105">Параметры</span><span class="sxs-lookup"><span data-stu-id="a3158-105">Parameters</span></span>  
 `bIInspectableOnly`  
 <span data-ttu-id="a3158-106">[in] Значение, указывающее, является ли метод будет возвращать только [!INCLUDE[wrt](../../../../includes/wrt-md.md)] интерфейсы (`IInspectable` интерфейсы) или все COM-интерфейсы, кэшируемых вызываемой оболочки времени выполнения (RCW).</span><span class="sxs-lookup"><span data-stu-id="a3158-106">[in] A value that indicates whether the method will return only [!INCLUDE[wrt](../../../../includes/wrt-md.md)] interfaces (`IInspectable` interfaces) or all COM interfaces that are cached by the runtime callable wrapper (RCW).</span></span>  
  
 `celt`  
 <span data-ttu-id="a3158-107">[in] Количество объектов, чьи адреса должны быть получены.</span><span class="sxs-lookup"><span data-stu-id="a3158-107">[in] The number of objects whose addresses are to be retrieved.</span></span>  
  
 `pceltFetched`  
 <span data-ttu-id="a3158-108">[out] Указатель на число `CORDB_ADDRESS` значения, фактически извлеченных в `ptrs`.</span><span class="sxs-lookup"><span data-stu-id="a3158-108">[out] A pointer to the number of `CORDB_ADDRESS` values actually returned in `ptrs`.</span></span>  
  
 `ptrs`  
 <span data-ttu-id="a3158-109">Указатель на начальный адрес массива `CORDB_ADDRESS` значений, содержащих адреса кэшированных объектов интерфейса.</span><span class="sxs-lookup"><span data-stu-id="a3158-109">A pointer to the starting address of an array of `CORDB_ADDRESS` values that contain the addresses of cached interface objects.</span></span>  
  
## <a name="remarks"></a><span data-ttu-id="a3158-110">Примечания</span><span class="sxs-lookup"><span data-stu-id="a3158-110">Remarks</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="a3158-111">Требования</span><span class="sxs-lookup"><span data-stu-id="a3158-111">Requirements</span></span>  
 <span data-ttu-id="a3158-112">**Платформы:** разделе [требования к системе для](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="a3158-112">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="a3158-113">**Заголовок:** CorDebug.idl, CorDebug.h</span><span class="sxs-lookup"><span data-stu-id="a3158-113">**Header:** CorDebug.idl, CorDebug.h</span></span>  
  
 <span data-ttu-id="a3158-114">**Библиотека:** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="a3158-114">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="a3158-115">**Версии платформы .NET framework:**[!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="a3158-115">**.NET Framework Versions:** [!INCLUDE[net_current_v45plus](../../../../includes/net-current-v45plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="a3158-116">См. также</span><span class="sxs-lookup"><span data-stu-id="a3158-116">See Also</span></span>  
 [<span data-ttu-id="a3158-117">Интерфейс ICorDebugComObjectValue</span><span class="sxs-lookup"><span data-stu-id="a3158-117">ICorDebugComObjectValue Interface</span></span>](../../../../docs/framework/unmanaged-api/debugging/icordebugcomobjectvalue-interface.md)  
 [<span data-ttu-id="a3158-118">Интерфейсы отладки</span><span class="sxs-lookup"><span data-stu-id="a3158-118">Debugging Interfaces</span></span>](../../../../docs/framework/unmanaged-api/debugging/debugging-interfaces.md)
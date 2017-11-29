---
title: "Метод ICorDebugManagedCallback::ControlCTrap"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-clr
ms.tgt_pltfrm: 
ms.topic: reference
api_name: ICorDebugManagedCallback.ControlCTrap
api_location: mscordbi.dll
api_type: COM
f1_keywords: ICorDebugManagedCallback::ControlCTrap
helpviewer_keywords:
- ICorDebugManagedCallback::ControlCTrap method [.NET Framework debugging]
- ControlCTrap method [.NET Framework debugging]
ms.assetid: 0500854e-2121-43d9-a028-64312da35258
topic_type: apiref
caps.latest.revision: "13"
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.openlocfilehash: df6aab0f8f220e53c8aab0d40a8b300d95822068
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2017
---
# <a name="icordebugmanagedcallbackcontrolctrap-method"></a><span data-ttu-id="7ca1d-102">Метод ICorDebugManagedCallback::ControlCTrap</span><span class="sxs-lookup"><span data-stu-id="7ca1d-102">ICorDebugManagedCallback::ControlCTrap Method</span></span>
<span data-ttu-id="7ca1d-103">Уведомляет отладчик о том, что сочетание клавиш CTRL + C, представленные в отлаживаемом процессе.</span><span class="sxs-lookup"><span data-stu-id="7ca1d-103">Notifies the debugger that a CTRL+C is trapped in the process that is being debugged.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="7ca1d-104">Синтаксис</span><span class="sxs-lookup"><span data-stu-id="7ca1d-104">Syntax</span></span>  
  
```  
HRESULT ControlCTrap (  
    [in] ICorDebugProcess *pProcess  
);  
```  
  
#### <a name="parameters"></a><span data-ttu-id="7ca1d-105">Параметры</span><span class="sxs-lookup"><span data-stu-id="7ca1d-105">Parameters</span></span>  
 `pProcess`  
 <span data-ttu-id="7ca1d-106">[in] Указатель на объект ICorDebugProcess, представляет собой процесс, в котором производится перехват CTRL + C.</span><span class="sxs-lookup"><span data-stu-id="7ca1d-106">[in] A pointer to an ICorDebugProcess object that represents the process in which the CTRL+C is trapped.</span></span>  
  
## <a name="return-value"></a><span data-ttu-id="7ca1d-107">Возвращаемое значение</span><span class="sxs-lookup"><span data-stu-id="7ca1d-107">Return Value</span></span>  
  
|<span data-ttu-id="7ca1d-108">HRESULT</span><span class="sxs-lookup"><span data-stu-id="7ca1d-108">HRESULT</span></span>|<span data-ttu-id="7ca1d-109">Описание</span><span class="sxs-lookup"><span data-stu-id="7ca1d-109">Description</span></span>|  
|-------------|-----------------|  
|<span data-ttu-id="7ca1d-110">S_OK</span><span class="sxs-lookup"><span data-stu-id="7ca1d-110">S_OK</span></span>|<span data-ttu-id="7ca1d-111">Отладчик будет обрабатывать нажатие клавиши CTRL + C.</span><span class="sxs-lookup"><span data-stu-id="7ca1d-111">The debugger will handle the CTRL+C trap.</span></span>|  
|<span data-ttu-id="7ca1d-112">S_FALSE</span><span class="sxs-lookup"><span data-stu-id="7ca1d-112">S_FALSE</span></span>|<span data-ttu-id="7ca1d-113">Отладчик не будет обрабатывать нажатие клавиши CTRL + C.</span><span class="sxs-lookup"><span data-stu-id="7ca1d-113">The debugger will not handle the CTRL+C trap.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="7ca1d-114">Примечания</span><span class="sxs-lookup"><span data-stu-id="7ca1d-114">Remarks</span></span>  
 <span data-ttu-id="7ca1d-115">Для этого обратного вызова, останавливаются все домены приложений в процессе.</span><span class="sxs-lookup"><span data-stu-id="7ca1d-115">All application domains within the process are stopped for this callback.</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="7ca1d-116">Требования</span><span class="sxs-lookup"><span data-stu-id="7ca1d-116">Requirements</span></span>  
 <span data-ttu-id="7ca1d-117">**Платформы:** разделе [требования к системе для](../../../../docs/framework/get-started/system-requirements.md).</span><span class="sxs-lookup"><span data-stu-id="7ca1d-117">**Platforms:** See [System Requirements](../../../../docs/framework/get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="7ca1d-118">**Заголовок:** CorDebug.idl, CorDebug.h</span><span class="sxs-lookup"><span data-stu-id="7ca1d-118">**Header:** CorDebug.idl, CorDebug.h</span></span>  
  
 <span data-ttu-id="7ca1d-119">**Библиотека:** CorGuids.lib</span><span class="sxs-lookup"><span data-stu-id="7ca1d-119">**Library:** CorGuids.lib</span></span>  
  
 <span data-ttu-id="7ca1d-120">**Версии платформы .NET framework:**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="7ca1d-120">**.NET Framework Versions:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="7ca1d-121">См. также</span><span class="sxs-lookup"><span data-stu-id="7ca1d-121">See Also</span></span>  
 [<span data-ttu-id="7ca1d-122">ICorDebugManagedCallback-интерфейс</span><span class="sxs-lookup"><span data-stu-id="7ca1d-122">ICorDebugManagedCallback Interface</span></span>](../../../../docs/framework/unmanaged-api/debugging/icordebugmanagedcallback-interface.md)
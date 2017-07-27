---
title: "Thread Pool ETW Events | Microsoft Docs"
ms.custom: ""
ms.date: "03/30/2017"
ms.prod: ".net-framework"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "dotnet-clr"
ms.tgt_pltfrm: ""
ms.topic: "article"
helpviewer_keywords: 
  - "thread pool events [.NET Framework]"
  - "ETW, thread pool events (CLR)"
ms.assetid: f2a21e3a-3b6c-4433-97f3-47ff16855ecc
caps.latest.revision: 8
author: "mairaw"
ms.author: "mairaw"
manager: "wpickett"
caps.handback.revision: 8
---
# Thread Pool ETW Events
<a name="top"></a> Эти события собирают сведения о рабочих потоках и потоках ввода\-вывода.  
  
 Имеется две группы событий пула потоков:  
  
-   [события пула рабочих потоков](#worker), предоставляющие информацию о способе использования приложением пула потоков и влиянии рабочих нагрузок на управление параллелизмом;  
  
-   [события пула потоков ввода\-вывода](#io), предоставляющие информацию о потоках ввода\-вывода, создаваемых, завершаемых, повторно активируемых или прерываемых в пуле потоков.  
  
<a name="worker"></a>   
## События пула рабочих потоков  
 Эти события связаны с пулом рабочих потоков среды выполнения и содержат уведомления о событиях потоков \(например, когда поток создается или останавливается\). Пул рабочих потоков использует адаптивный алгоритм для управления параллелизмом, в соответствии с которым число потоков рассчитывается в зависимости от измеренной пропускной способности. С помощью событий пула рабочих потоков можно понять, как приложение использует пул потоков, и оценить влияние конкретных нагрузок на управление параллелизмом.  
  
### ThreadPoolWorkerThreadStart и ThreadPoolWorkerThreadStop  
 В таблице ниже показаны ключевые слова и уровни для этих событий. \(Дополнительные сведения см. в разделе [CLR ETW Keywords and Levels](../../../docs/framework/performance/clr-etw-keywords-and-levels.md).\)  
  
|Ключевое слово для вызова события|Уровень|  
|---------------------------------------|-------------|  
|`ThreadingKeyword` \(0x10000\)|Информационный \(4\)|  
  
 В таблице ниже представлены сведения о событии.  
  
||||  
|-|-|-|  
|Событие|Идентификатор события|Условие вызова|  
|`ThreadPoolWorkerThreadStart`|50|Создается рабочий поток.|  
|`ThreadPoolWorkerThreadStop`|51|Рабочий поток останавливается.|  
|`ThreadPoolWorkerThreadRetirementStart`|52|Рабочий поток завершается.|  
|`ThreadPoolWorkerThreadRetirementStop`|53|Завершенный рабочий поток снова становится активным.|  
  
 В таблице ниже представлены данные события.  
  
|Имя поля|Тип данных|Описание|  
|--------------|----------------|--------------|  
|ActiveWorkerThreadCount|win:UInt32|Число рабочих потоков, доступных для выполнения процесса, включая уже работающие потоки.|  
|RetiredWorkerThreadCount|win:UInt32|Число рабочих потоков, которые недоступны для выполнения процесса, но находятся в резерве на случай, если позже понадобятся дополнительные потоки.|  
|ClrInstanceID|Win:UInt16|Уникальный идентификатор экземпляра CLR или CoreCLR.|  
  
### ThreadPoolWorkerThreadAdjustment  
 Эти события пула потоков содержат сведения для понимания и отладки поведения алгоритма вставки потока \(управления параллелизмом\). Эти сведения используются внутри пула рабочих потоков.  
  
#### ThreadPoolWorkerThreadAdjustmentSample  
 В таблице ниже показаны ключевое слово и уровень.  
  
|Ключевое слово для вызова события|Уровень|  
|---------------------------------------|-------------|  
|`ThreadingKeyword` \(0x10000\)|Информационный \(4\)|  
  
 В таблице ниже представлены сведения о событии.  
  
|Событие|Идентификатор события|Описание|  
|-------------|---------------------------|--------------|  
|`ThreadPoolWorkerThreadAdjustmentSample`|54|Указывает на сбор сведений для одного образца, то есть измерение пропускной способности с определенным уровнем параллелизма в некоторый момент времени.|  
  
 В таблице ниже представлены данные события.  
  
|Имя поля|Тип данных|Описание|  
|--------------|----------------|--------------|  
|Пропускная способность|win:Double|Число завершений в единицу времени.|  
|ClrInstanceID|Win:UInt16|Уникальный идентификатор экземпляра CLR или CoreCLR.|  
  
#### ThreadPoolWorkerThreadAdjustmentAdjustment  
 В таблице ниже показаны ключевое слово и уровень.  
  
|Ключевое слово для вызова события|Уровень|  
|---------------------------------------|-------------|  
|`ThreadingKeyword` \(0x10000\)|Информационный \(4\)|  
  
 В таблице ниже представлены сведения о событии.  
  
|Событие|Идентификатор события|Описание|  
|-------------|---------------------------|--------------|  
|`ThreadPoolWorkerThreadAdjustmentAdjustment`|55|Регистрирует изменение в управлении, когда алгоритм вставки потока \(поиск экстремума\) определяет, что произошло изменение уровня параллелизма.|  
  
 В таблице ниже представлены данные события.  
  
|Имя поля|Тип данных|Описание|  
|--------------|----------------|--------------|  
|AverageThroughput|win:Double|Средняя пропускная способность образца измерений.|  
|NewWorkerThreadCount|win:UInt32|Новое число активных рабочих потоков.|  
|Причина|win:UInt32|Причина корректировки:<br /><br /> 0x00 — прогрев;<br /><br /> 0x01 — инициализация;<br /><br /> 0x02 — случайное движение;<br /><br /> 0x03 — движение вверх;<br /><br /> 0x04 — точка изменения;<br /><br /> 0x05 — стабилизация;<br /><br /> 0x06 — перегрузка;<br /><br /> 0x07 — истекло время ожидания потока.|  
|ClrInstanceID|Win:UInt16|Уникальный идентификатор экземпляра CLR или CoreCLR.|  
  
#### ThreadPoolWorkerThreadAdjustmentStats  
 В таблице ниже показаны ключевое слово и уровень.  
  
|Ключевое слово для вызова события|Уровень|  
|---------------------------------------|-------------|  
|`ThreadingKeyword` \(0x10000\)|Информационный \(4\)|  
  
 В таблице ниже представлены сведения о событии.  
  
|Событие|Идентификатор события|Описание|  
|-------------|---------------------------|--------------|  
|`ThreadPoolWorkerThreadAdjustmentStats`|56|Собирает данные по пулу потоков.|  
  
 В таблице ниже представлены данные события.  
  
|Имя поля|Тип данных|Описание|  
|--------------|----------------|--------------|  
|Длительность|win:Double|Время в секундах, в течение которого выполнялся сбор статистики.|  
|Пропускная способность|win:Double|Среднее число завершений в секунду в течение данного интервала.|  
|ThreadWave|win:Double|Зарезервировано для внутреннего использования.|  
|ThroughputWave|win:Double|Зарезервировано для внутреннего использования.|  
|ThroughputErrorEstimate|win:Double|Зарезервировано для внутреннего использования.|  
|AverageThroughputErrorEstimate|win:Double|Зарезервировано для внутреннего использования.|  
|ThroughputRatio|win:Double|Относительное улучшение пропускной способности, вызванное изменениями числа активных рабочих потоков в течение этого интервала.|  
|Confidence|win:Double|Мера обоснованности поля ThroughputRatio.|  
|NewcontrolSetting|win:Double|Число активных рабочих потоков, которое будет служить основой для будущих изменений числа активных потоков.|  
|NewThreadWaveMagnitude|Win:UInt16|Величина будущих изменений числа активных потоков.|  
|ClrInstanceID|Win:UInt16|Уникальный идентификатор экземпляра CLR или CoreCLR.|  
  
 [К началу](#top)  
  
<a name="io"></a>   
## События потоков ввода\-вывода  
 Эти события пула потоков создаются для потоков в пуле потоков ввода\-вывода \(порты завершения\), который является асинхронным.  
  
### IOThreadCreate\_V1  
 В таблице ниже показаны ключевое слово и уровень.  
  
|Ключевое слово для вызова события|Уровень|  
|---------------------------------------|-------------|  
|`ThreadingKeyword` \(0x10000\)|Информационный \(4\)|  
  
 В таблице ниже представлены сведения о событии.  
  
||||  
|-|-|-|  
|Событие|Идентификатор события|Условие вызова|  
|`IOThreadCreate_V1`|44|Поток ввода\-вывода создается в пуле потоков.|  
  
 В таблице ниже представлены данные события.  
  
|Имя поля|Тип данных|Описание|  
|--------------|----------------|--------------|  
|Счетчик|win:UInt64|Число потоков ввода\-вывода, включая вновь созданный поток.|  
|NumRetired|win:UInt64|Число завершенных рабочих потоков.|  
|ClrInstanceID|Win:UInt16|Уникальный идентификатор экземпляра CLR или CoreCLR.|  
  
### IOThreadRetire\_V1  
 В таблице ниже показаны ключевое слово и уровень.  
  
|Ключевое слово для вызова события|Уровень|  
|---------------------------------------|-------------|  
|`ThreadingKeyword` \(0x10000\)|Информационный \(4\)|  
  
 В таблице ниже представлены сведения о событии.  
  
|Событие|Идентификатор события|Условие вызова|  
|-------------|---------------------------|--------------------|  
|`IOThreadRetire_V1`|46|Поток ввода\-вывода становится кандидатом на завершение.|  
  
 В таблице ниже представлены данные события.  
  
|Имя поля|Тип данных|Описание|  
|--------------|----------------|--------------|  
|Счетчик|win:UInt64|Число потоков ввода\-вывода, остающихся в пуле потоков.|  
|NumRetired|win:UInt64|Число завершенных потоков ввода\-вывода.|  
|ClrInstanceID|Win:UInt16|Уникальный идентификатор экземпляра CLR или CoreCLR.|  
  
### IOThreadUnretire\_V1  
 В таблице ниже показаны ключевое слово и уровень.  
  
|Ключевое слово для вызова события|Уровень|  
|---------------------------------------|-------------|  
|`ThreadingKeyword` \(0x10000\)|Информационный \(4\)|  
  
 В таблице ниже представлены сведения о событии.  
  
|Событие|Идентификатор события|Условие вызова|  
|-------------|---------------------------|--------------------|  
|`IOThreadUnretire_V1`|47|Завершенный поток ввода\-вывода повторно активируется из\-за ввода\-вывода, поступающего в течение периода ожидания после того, как поток стал кандидатом на завершение.|  
  
 В таблице ниже представлены данные события.  
  
|Имя поля|Тип данных|Описание|  
|--------------|----------------|--------------|  
|Счетчик|win:UInt64|Число потоков ввода\-вывода в пуле потоков, включая этот поток.|  
|NumRetired|win:UInt64|Число завершенных потоков ввода\-вывода.|  
|ClrInstanceID|Win:UInt16|Уникальный идентификатор экземпляра CLR или CoreCLR.|  
  
### IOThreadTerminate  
 В таблице ниже показаны ключевое слово и уровень.  
  
|Ключевое слово для вызова события|Уровень|  
|---------------------------------------|-------------|  
|`ThreadingKeyword` \(0x10000\)|Информационный \(4\)|  
  
 В таблице ниже представлены сведения о событии.  
  
|Событие|Идентификатор события|Условие вызова|  
|-------------|---------------------------|--------------------|  
|`IOThreadTerminate`|45|Поток ввода\-вывода создается в пуле потоков.|  
  
 В таблице ниже представлены данные события.  
  
|Имя поля|Тип данных|Описание|  
|--------------|----------------|--------------|  
|Счетчик|win:UInt64|Число потоков ввода\-вывода, остающихся в пуле потоков.|  
|NumRetired|win:UInt64|Число завершенных потоков ввода\-вывода.|  
|ClrInstanceID|Win:UInt16|Уникальный идентификатор экземпляра CLR или CoreCLR.|  
  
## См. также  
 [CLR ETW Events](../../../docs/framework/performance/clr-etw-events.md)
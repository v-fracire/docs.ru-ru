---
title: '&lt;прослушиватели&gt; элемент для &lt;трассировки&gt;'
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/trace/listeners
helpviewer_keywords:
- <listeners> element
- listeners element
ms.assetid: 1394c2c3-6304-46db-87c1-8e8b16f5ad5b
author: mcleblanc
ms.author: markl
ms.openlocfilehash: bfcf96c553f85aeb0a40dfd6ea36667d504e8eee
ms.sourcegitcommit: ea00c05e0995dae928d48ead99ddab6296097b4c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2018
ms.locfileid: "48028005"
---
# <a name="ltlistenersgt-element-for-lttracegt"></a>&lt;прослушиватели&gt; элемент для &lt;трассировки&gt;
Указывает прослушиватель, который собирает, хранилищ и направляет сообщения. Прослушиватели направляют выходные данные трассировки соответствующему целевому объекту.  
  
 \<Конфигурация > элемент  
\<System.Diagnostics > элемент  
\<трассировки > элемент  
\<прослушиватели > элемент для \<трассировки >  
  
## <a name="syntax"></a>Синтаксис  
  
```xml  
<listeners>   
  <add>...</add>  
  <clear/>  
  <remove ... />  
</listeners>  
```  
  
## <a name="attributes-and-elements"></a>Атрибуты и элементы  
 В следующих разделах описаны атрибуты, дочерние и родительские элементы.  
  
### <a name="attributes"></a>Атрибуты  
 Отсутствует.  
  
### <a name="child-elements"></a>Дочерние элементы  
  
|Элемент|Описание|  
|-------------|-----------------|  
|[\<add>](../../../../../docs/framework/configure-apps/file-schema/trace-debug/add-element-for-listeners-for-trace.md)|Добавляет прослушиватель в коллекцию `Listeners`.|  
|[\<clear>](../../../../../docs/framework/configure-apps/file-schema/trace-debug/clear-element-for-listeners-for-trace.md)|Очищает коллекцию `Listeners` для трассировки.|  
|[\<remove>](../../../../../docs/framework/configure-apps/file-schema/trace-debug/remove-element-for-listeners-for-trace.md)|Удаляет прослушиватель из `Listeners` коллекции.|  
  
### <a name="parent-elements"></a>Родительские элементы  
  
|Элемент|Описание|  
|-------------|-----------------|  
|`configuration`|Корневой элемент в любом файле конфигурации, используемом средой CLR и приложениями .NET Framework.|  
|`system.diagnostics`|Задает корневой элемент для раздела конфигурации ASP.NET.|  
|`trace`|Содержит прослушиватели, которые собирают, хранят и маршрутизируют сообщения трассировки.|  
  
## <a name="remarks"></a>Примечания  
 <xref:System.Diagnostics.Debug> И <xref:System.Diagnostics.Trace> классы одного и того же **прослушиватели** коллекции. Если добавить объект прослушивателя в коллекцию в одном из этих классов, другой класс использует этот же прослушиватель. Классы прослушивателя, поставлявшихся с .NET Framework являются производными от <xref:System.Diagnostics.TraceListener> класса.  
  
## <a name="configuration-file"></a>Файл конфигурации  
 Этот элемент может использоваться в файле конфигурации компьютера (Machine.config) и файле конфигурации приложения.  
  
## <a name="example"></a>Пример  
 В следующем примере показано, как использовать  **\<прослушиватели >** элемент для добавления прослушивателей `MyListener` и `MyEventListener` для **прослушиватели** коллекции. `MyListener` Создает файл с именем `MyListener.log` и записывает выходные данные в файл. `MyEventListener` создает запись в журнале событий.  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <trace autoflush="true" indentsize="0">  
      <listeners>  
        <add name="myListener"   
          type="System.Diagnostics.TextWriterTraceListener,   
            system, version=1.0.3300.0, Culture=neutral,   
            PublicKeyToken=b77a5c561934e089"   
          initializeData="c:\myListener.log" />  
        <add name="MyEventListener"  
          type="System.Diagnostics.EventLogTraceListener,   
            system, version=1.0.3300.0, Culture=neutral,   
            PublicKeyToken=b77a5c561934e089"  
          initializeData="MyConfigEventLog"/>  
      </listeners>  
    </trace>  
  </system.diagnostics>  
</configuration>  
```  
  
## <a name="see-also"></a>См. также  
 <xref:System.Diagnostics.TraceListener>  
 [Схема параметров трассировки и отладки](../../../../../docs/framework/configure-apps/file-schema/trace-debug/index.md)

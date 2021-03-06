---
title: Общие сведения об интеграции с приложениями COM+
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, COM+ integration
- WCF, COM+ integration
ms.assetid: e481e48f-7096-40eb-9f20-7f0098412941
ms.openlocfilehash: 155365c72fd3f5915db12104f45a500f3176f67b
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
ms.locfileid: "33496329"
---
# <a name="integrating-with-com-applications-overview"></a>Общие сведения об интеграции с приложениями COM+
Windows Communication Foundation (WCF) предоставляет среду с широкими возможностями для создания распределенных приложений. Если вы уже используете логики компонентно ориентированного приложения, размещенного в COM +, WCF можно использовать для расширения существующей логики вместо ее переписывания. Стандартный сценарий - предоставление доступа существующему приложению COM+ или бизнес-логике Enterprise Services службы через веб-службы.  
  
 Когда доступ к компоненту COM+ предоставляется через веб-службу, спецификация и контракт таких служб определяются путем автоматического сопоставления, которое выполняется во время инициализации приложения. В следующем списке показана концептуальная модель такого сопоставления:  
  
-   для каждого из классов COM, к которым предоставляется доступ, определяется одна служба;  
  
-   контракт для службы получается напрямую из определения интерфейса выбранного компонента с определенной в конфигурации возможностью исключения определенных методов;  
  
-   операции в контракте получаются напрямую из методов в определении интерфейса компонента;  
  
-   параметры этих операций получаются напрямую из типа COM-взаимодействия, который соответствует параметрам метода компонента.  
  
 Адреса и привязки транспорта по умолчанию для службы задаются в файле конфигурации службы, но при необходимости их можно изменить.  
  
> [!NOTE]
>  Контракты для предоставляемых веб-служб остаются постоянными, если только не изменяются интерфейсы COM+ и конфигурация. Изменение нескольких интерфейсов не приводит к автоматическому обновлению доступных служб и требует повторного запуска программы настройки модели служб COM+ (ComSvcConfig.exe).  
  
 Требования проверки подлинности и авторизации приложения COM+ и его компонентов остаются в силе и при использовании приложения через веб-службу.  
  
 Если вызывающий объект инициирует транзакцию веб-службы, компоненты, помеченные в качестве транзакционных, включатся в список в области транзакции.  
  
 Чтобы предоставить доступ к интерфейсу компонента COM+ через веб-службу без изменения компонента, необходимо выполнить следующие действия.  
  
1.  Определите, можно ли предоставить доступ к интерфейсу компонента COM+ через веб-службу.  
  
2.  Выберите подходящий режим размещения.  
  
3.  С помощью средства настройки модели службы COM+ (ComSvcConfig.exe) добавьте веб-службу для нужного интерфейса. Дополнительные сведения об использовании ComSvcConfig.exe см. в разделе [как: использование программы настройки модели служб COM +](../../../../docs/framework/wcf/feature-details/how-to-use-the-com-service-model-configuration-tool.md).  
  
4.  Задайте необходимые дополнительные параметры службы в файле конфигурации приложения. Дополнительные сведения о настройке компонентов см. в разделе [как: COM + настройки параметров службы](../../../../docs/framework/wcf/feature-details/how-to-configure-com-service-settings.md).  
  
## <a name="supported-interfaces"></a>Поддерживаемые интерфейсы  
 На интерфейсы, которые можно делать доступными через веб-службы, накладываются определенные ограничения. Следующие типы интерфейсов не поддерживаются:  
  
-   интерфейсы, которые передают в качестве параметров ссылки на объекты; ограниченный подход к передаче ссылок на объекты описан в разделе "Ограниченная поддержка ссылок на объекты";  
  
-   интерфейсы, которые передают типы, несовместимые с преобразованиями COM-взаимодействия [!INCLUDE[dnprdnshort](../../../../includes/dnprdnshort-md.md)];  
  
-   интерфейсы приложений, для которых при размещении в COM+ включено объединение приложений в пул;  
  
-   интерфейсы компонентов, которые помечены в приложении как закрытые;  
  
-   интерфейсы инфраструктуры COM+;  
  
-   интерфейсы системных приложений;  
  
-   интерфейсы компонентов Enterprise Services, не включенные в глобальный кэш сборок.  
  
### <a name="limited-object-reference-support"></a>Ограниченная поддержка ссылок на объекты  
 Поскольку некоторые развернутые компоненты COM+ используют объекты, передаваемые по ссылкам, например при возврате объекта ADO Recordset, интеграция с COM+ включает ограниченную поддержку передачи параметров, являющихся ссылками на объекты. Эта поддержка ограничивается объектами, которые реализуют интерфейс COM `IPersistStream`. Она включает объекты ADO Recordset и может быть реализована для COM-объектов, относящихся к конкретному приложению.  
  
 Для включения такой поддержки у программы ComSvcConfig.exe имеется **allowreferences** коммутатора, который отключает обычный параметр подписи методов и проверяет, что выполняется эта программы, чтобы убедиться, что параметры ссылки на объект не используются . Кроме того, типы объектов, передаваемых в качестве параметров, должны быть указаны и определены в элементе конфигурации <`persistableTypes`>, который является дочерним для элемента <`comContract`>.  
  
 Если включен этот режим, служба интеграции COM+ использует для сериализации и десериализации экземпляров объектов интерфейс `IPersistStream`. Если экземпляр объекта не поддерживает интерфейс `IPersistStream`, создается исключение.  
  
 Внутри клиентского приложения для передачи объектов службе и возврата объектов можно использовать методы объекта <xref:System.ServiceModel.ComIntegration.PersistStreamTypeWrapper>.  
  
> [!NOTE]
>  Из-за особенностей пользовательские и платформой подход сериализацию это наилучшим образом подходит для использования между клиентами WCF и службы WCF.  
  
## <a name="selecting-the-hosting-mode"></a>Выбор режима размещения  
 Модель COM+ предоставляет доступ через веб-службы с использованием одного из следующих режимов размещения.  
  
-   Размещение в службах COM+  
  
     Веб-служба размещается в выделенном процессе приложения на сервере COM+ (Dllhost.exe). При использовании этого режима приложение должно быть явным образом запущено, прежде чем оно сможет получать запросы веб-службы. Чтобы избежать перехода приложения и его службы в режим ожидания, можно использовать параметры COM+ "Запустить как службу NT" или "Не останавливать при ожидании". В этом режиме обращаться к приложению можно как через веб-службу, так и через доступ DCOM.  
  
-   Размещение на веб-сервере  
  
     Веб-служба размещается в рабочем процессе веб-сервера. В этом режиме служба COM+ не обязательно должна быть активной при получении первого запроса. Если приложение неактивно при получении первого запроса, оно автоматически активируется перед обработкой запроса. В этом режиме доступ к серверному приложению также может осуществляться через веб-службу или технологию DCOM, однако при обработке запросов веб-службы будет происходить переход процесса. Обычно для этого необходимо включить олицетворение на клиенте. В WCF, это можно сделать с помощью <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> свойство <xref:System.ServiceModel.Security.WindowsClientCredential> класса, к которому можно получить в качестве свойства универсального <xref:System.ServiceModel.ChannelFactory%601> класса, а также <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation> значение перечисления.  
  
-   Внутрипроцессное размещение на веб-сервере  
  
     Веб-служба и логика приложения COM+ размещаются внутри рабочего процесса веб-сервера. Это позволяет автоматически активировать режим размещения на веб-сервере без перехода процесса при запросах веб-службы. Недостаток такого подхода заключается в том, что к серверному приложению невозможно обратиться с помощью технологии DCOM.  
  
### <a name="security-considerations"></a>Вопросы безопасности  
 Как и другие службы WCF параметры безопасности для предоставленной службы осуществляется с помощью параметров конфигурации для канала WCF. Обычные параметры безопасности DCOM, например разрешения DCOM уровня компьютера, не действуют. Чтобы включить роли приложений COM+, для компонента необходимо включить режим авторизации "Принудительная проверка доступа на уровне компонента".  
  
 При использовании незащищенной привязки возможно искажение передаваемых данных или раскрытие информации. Во избежание этого рекомендуется использовать защищенную привязку.  
  
 При размещении в служба COM+и на веб-сервере клиентское приложение должно разрешать серверному процессу олицетворять клиентского пользователя. Это можно сделать в клиентов WCF, задав уровень олицетворения <xref:System.Security.Principal.TokenImpersonationLevel.Impersonation>.  
  
 Если используются службы IIS или службы активации Windows (WAS) с транспортом HTTP, можно с помощью средства Httpcfg.exe зарезервировать адрес конечной точки транспорта. В других конфигурациях важно обеспечить защиту от поддельных служб, которые выдают себя за настоящие службы. Чтобы избежать запуска поддельной службы в выбранной конечной точке, можно настроить настоящую службу на выполнение в качестве службы NT. В результате настоящая служба утвердит адрес конечной точки до того, как это сможет сделать какая-либо поддельная служба.  
  
 При размещении приложения COM + с настроенными ролями COM + как службу веб сервере, «Запустите IIS учетной записи процесса» должен добавляться к одной из ролей приложения. Эта учетная запись (обычно она имеет имя IWAM_имя_компьютера) должна добавляться, чтобы можно было безопасно отключить объекты после использования. Не следует предоставлять этой учетной записи какие-либо дополнительные разрешения.  
  
 Функции перезапуска процессов COM+ невозможно использовать в интегрированных приложениях. Если приложение настроено на использование перезапуска процессов, а компоненты выполняются в процессе служб COM+, запустить службу не удастся. Это требование не распространяется на службы, использующие внутрипроцессное размещение на веб-сервере, поскольку параметры перезапуска процессов в этом случае не действуют.  
  
## <a name="see-also"></a>См. также  
 [Общие сведения об интеграции с приложениями COM](../../../../docs/framework/wcf/feature-details/integrating-with-com-applications-overview.md)

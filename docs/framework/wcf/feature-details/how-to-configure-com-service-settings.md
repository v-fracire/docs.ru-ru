---
title: Практическое руководство. Настройка параметров службы COM+
ms.date: 03/30/2017
helpviewer_keywords:
- COM+ [WCF], configuring service settings
ms.assetid: f42a55a8-3af8-4394-9fdd-bf12a93780eb
ms.openlocfilehash: 9cbeb03e21ed3b8ec272d47815ac7e9c48d77499
ms.sourcegitcommit: fd8d4587cc26e53f0e27e230d6e27d828ef4306b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348774"
---
# <a name="how-to-configure-com-service-settings"></a>Практическое руководство. Настройка параметров службы COM+
Если интерфейс приложения добавляется или удаляется с использованием средства конфигурации служб COM+, конфигурация веб-службы обновляется в файле конфигурации приложения. В режиме размещенного в COM + файл Application.config помещается в корневом каталоге приложения (приложения %PROGRAMFILES%\ComPlus\\{appid} значение по умолчанию). В обоих режимах размещения на веб-сервере файл Web.config помещается в заданный виртуальный корневой каталог.  
  
> [!NOTE]
>  Для защиты от подделки сообщений, обмен которыми выполняется между клиентом и сервером, необходимо использовать подписывание сообщения. Кроме того, для защиты от раскрытия информации, содержащейся в сообщениях, обмен которыми выполняется между клиентом и сервером, следует использовать шифрование на уровне сообщения или транспорта. Как и в случае со службами Windows Communication Foundation (WCF), следует использовать регулирования для ограничения числа одновременных вызовов, подключений, экземпляров и отложенных операций. Это помогает предотвратить чрезмерное потребление ресурсов. Поведение регулирования задается параметрами файла конфигурации службы.  
  
## <a name="example"></a>Пример  
 Рассмотрим компонент, реализующий следующий интерфейс.  
  
```  
[Guid("C551FBA9-E3AA-4272-8C2A-84BD8D290AC7")]  
public interface IFinances  
{  
    string Debit(string accountNo, double amount);  
    string Credit(string accountNo, double amount);  
}  
```  
  
 Если компонент предоставляется как веб-служба, соответствующий предоставляемый контракт службы, которому должны соответствовать клиенты, выглядит следующим образом.  
  
```  
[ServiceContract(Session = true,  
Namespace = "http://tempuri.org/C551FBA9-E3AA-4272-8C2A-84BD8D290AC7",  
Name = "IFinances")]  
public interface IFinancesContract : IDisposable  
{  
    [OperationContract]  
    string Debit(string accountNo, double amount);  
    [OperationContract]  
    string Credit(string accountNo, double amount);  
}  
```  
  
> [!NOTE]
>  Идентификатор интерфейса формирует часть исходного пространства имен для контракта.  
  
 Клиентские приложения, использующие эту службу, должны соответствовать этому контракту, а также использовать привязку, совместимую с привязкой, заданной в конфигурации приложения.  
  
 В следующем примере кода показан файл конфигурации по умолчанию. Будучи Windows Communication Foundation (WCF) веб-службы, это соответствует схеме конфигурации модели служб standard и могут редактироваться в так же, как другие файлы конфигурации служб WCF.  
  
 Типичные изменения включают следующее.  
  
- Изменение адреса конечной точки с формы ApplicationName/ComponentName/InterfaceName по умолчанию на более удобную форму.  
  
- Изменение пространства имен службы по умолчанию `http://tempuri.org/InterfaceID` форму, чтобы более подходящую форму.  
  
- Изменение настройки конечной точки, чтобы она использовала другую привязку транспорта.  
  
     В случае размещения в COM+ по умолчанию используется транспорт именованных каналов, но вместо него можно использовать транспорт вне компьютера, такой как TCP.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
    <system.serviceModel>  
        <bindings>  
            <netNamedPipeBinding>  
                <binding name="comNonTransactionalBinding" />  
                <binding name="comTransactionalBinding" transactionFlow="true" />  
            </netNamedPipeBinding>  
        </bindings>  
        <comContracts>  
            <comContract contract="{C551FBA9-E3AA-4272-8C2A-84BD8D290AC7}"  
                name="IFinances" namespace="http://tempuri.org/C551FBA9-E3AA-4272-8C2A-84BD8D290AC7"  
                requiresSession="true">  
                <exposedMethods>  
                    <add exposedMethod="Debit" />  
                    <add exposedMethod="Credit" />  
                </exposedMethods>  
            </comContract>  
        </comContracts>  
        <services>  
            <service name="{DCDB24CC-0B19-4534-95CD-FBBFF4D67DD9},{C942B840-AD54-4A44-B5F7-928130980AB9}">  
                <endpoint address="IFinances" binding="netNamedPipeBinding" bindingConfiguration="comNonTransactionalBinding"  
                    contract="{C551FBA9-E3AA-4272-8C2A-84BD8D290AC7}" />  
                <host>  
                    <baseAddresses>  
                        <add baseAddress="net.pipe://localhost/ServiceModelDocSampleApp/ServiceModelDocSample.esFinance" />  
                    </baseAddresses>  
                </host>  
            </service>  
        </services>  
    </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>См. также  
* [Интеграция с приложениями COM+](../../../../docs/framework/wcf/feature-details/integrating-with-com-plus-applications.md)

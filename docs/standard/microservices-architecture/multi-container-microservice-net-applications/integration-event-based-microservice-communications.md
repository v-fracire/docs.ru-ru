---
title: Реализация взаимодействия между микрослужбами на основе событий (события интеграции)
description: Архитектура микрослужб .NET для контейнерных приложений .NET | Реализация взаимодействия между микрослужбами на основе событий (события интеграции)
author: CESARDELATORRE
ms.author: wiwagn
ms.date: 12/11/2017
ms.openlocfilehash: 6a365c284d66ea24a9bb4caae51c63f22c79877b
ms.sourcegitcommit: c93fd5139f9efcf6db514e3474301738a6d1d649
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2018
ms.locfileid: "50194050"
---
# <a name="implementing-event-based-communication-between-microservices-integration-events"></a>Реализация взаимодействия между микрослужбами на основе событий (события интеграции)

Как было описано ранее, при обмене данными на основе событий микрослужба публикует событие, когда происходит что-то важное, например, когда она обновляет бизнес-объект. Другие микрослужбы подписываются на эти события. Когда микрослужба получает событие, она может обновить свои собственные бизнес-объекты, что может привести к публикации дополнительных событий. Эта система публикаций и подписок обычно реализуется с помощью шины событий. Шина событий может быть спроектирована как интерфейс с API, который необходим для подписки и отмены подписки на события и для публикации событий. Шина событий может иметь одну или несколько реализаций на основе любого межпроцессорного обмена или обмена сообщениями, например очереди сообщений или служебной шины, поддерживающей асинхронное взаимодействие и модель публикаций и подписок.

События можно использовать для реализации бизнес-транзакций, которые охватывают несколько служб, что позволяет обеспечить итоговую согласованность между этими службами. Согласованная по принципу итоговой согласованности транзакция состоит из коллекции распределенных действий. Для каждого действия микрослужба обновляет бизнес-объект и публикует событие, которое вызывает следующее действие.

![](./media/image19.PNG)

**Рис. 8-18**. Обмен данными под управлением событиями на основе шины событий

В этом разделе описано, как реализовать такой обмен данными в .NET с помощью общего интерфейса шины событий, как показано на рис. 8-18. Существует несколько потенциальных реализаций, в каждой из которых используются собственные технологии или инфраструктуры, например RabbitMQ, служебная шина Azure или любые другие сторонние реализации служебной шины, коммерческие или с открытым исходным кодом.

## <a name="using-message-brokers-and-services-buses-for-production-systems"></a>Использование брокеров сообщений и служебных шин для систем в рабочей среде

Как упоминалось в разделе об архитектуре, для реализации абстрактной шины событий можно выбрать одну из нескольких технологий обмена сообщениями. Однако эти технологии находятся на разных уровнях. Например, RabbitMQ, транспорт брокера обмена сообщениями, находится на более низком уровне по сравнению с коммерческими продуктами, такими как служебная шина Azure, NServiceBus, MassTransit или Brighter. Большинство этих продуктов могут работать поверх RabbitMQ или служебной шины Azure. Выбор продукта зависит от количества функций и объема масштабирования, необходимых для вашего приложения.

Для реализации шины событий, предназначенной только для демонстрации принципа работы и используемой в среде разработки, как было показано на примере eShopOnContainers, достаточно простой реализации на основе системы RabbitMQ, работающей в контейнере. Однако для создания критически важных систем и систем в рабочей среде с высокой масштабируемостью может потребоваться оценить и использовать служебную шину Azure.

Если вам необходима абстракция более высокого уровня и более широкий набор функций, например [Sagas](https://docs.particular.net/nservicebus/sagas/) для длительных рабочих процессов, которые упрощают распределенную разработку, можете воспользоваться другими коммерческими и открытыми реализациями служебной шины, например NServiceBus, MassTransit и Brighter. В этом случае обычно используются не ваши собственные абстракции, а абстракции и API, предоставляемые этими решениями высокого уровня (например, [абстракции простой шины событий в eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/BuildingBlocks/EventBus/EventBus/Abstractions/IEventBus.cs)). Поэтому можете обратиться к [вилке проекта eShopOnContainers, в которой используется NServiceBus](https://go.particular.net/eShopOnContainers) (дополнительный производный пример, реализованный Particular Software)

Конечно, вы всегда можете создать собственные функции служебной шины на основе технологий низкого уровня, таких как RabbitMQ и Docker, но объем работы, необходимой для того, чтобы "изобрести велосипед", может быть слишком большим для пользовательского корпоративного приложения.

## <a name="integration-events"></a>События интеграции

События интеграции используются для синхронизации состояния домена между несколькими микрослужбами или внешними системами. Это достигается путем публикации событий интеграции за границы микрослужбы. При публикации события в нескольких микрослужбах-получателях (в тех микрослужбах, которые подписаны на события интеграции), соответствующий обработчик событий в каждой микрослужбе-получателе обрабатывает событие.

Событие интеграции по сути представляет собой класс для хранения данных, как показано в следующем примере:

```csharp
public class ProductPriceChangedIntegrationEvent : IntegrationEvent
{
    public int ProductId { get; private set; }
    public decimal NewPrice { get; private set; }
    public decimal OldPrice { get; private set; }

    public ProductPriceChangedIntegrationEvent(int productId, decimal newPrice,
        decimal oldPrice)
    {
        ProductId = productId;
        NewPrice = newPrice;
        OldPrice = oldPrice;
    }
}
```

События интеграции можно определить на уровне приложения для каждой микрослужбы, поэтому они отделены от других микрослужб примерно так же, как модели представления разделены на сервере и на клиенте. Не рекомендуется использовать общую библиотеку событий интеграции для нескольких микрослужб; это может привести к связыванию этих микрослужб с одной библиотекой данных для определения событий. Это нежелательно по тем же причинам, по которым нежелательно использовать общую модель предметной области для нескольких микрослужб — микрослужбы должны быть полностью автономными.

Существует всего несколько видов библиотек, которые должны быть общими для микрослужб. Одна из таких библиотек — конечные блоки приложений, например [API клиента шины событий](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/BuildingBlocks/EventBus), как в eShopOnContainers. Другие — библиотеки, предоставляющие инструменты, которые можно совместно использовать как компоненты NuGet, например сериализаторы JSON.

## <a name="the-event-bus"></a>Шина событий

Шина событий позволяет осуществлять обмен данными между микрослужбами с помощью механизма публикации и подписки, при котором компоненты могут явно не взаимодействовать друг с другом, как показано на рис. 8-19.

![](./media/image20.png)

**Рис. 8-19**. Основные сведения о шине событий с использованием механизма публикации и подписки

Шина событий связана с шаблоном наблюдателя и с шаблоном публикации и подписки.

### <a name="observer-pattern"></a>Шаблон наблюдателя

В [шаблоне наблюдателя](https://en.wikipedia.org/wiki/Observer_pattern) основной объект (наблюдаемый объект) передает другим объектам (наблюдателям) соответствующие данные (события).

### <a name="publish-subscribe-pubsub-pattern"></a>Шаблон публикации и подписки 

Назначение [шаблона публикации и подписки](https://msdn.microsoft.com/library/ff649664.aspx) — точно такое же, как у шаблона наблюдателя: уведомлять другие службы при возникновении определенных событий. Однако между шаблоном наблюдателя и шаблоном публикации и подписки есть важное различие. В шаблоне наблюдателя широковещательная рассылка осуществляется напрямую из наблюдаемого объекта наблюдателям, поэтому они "знают" друг друга. При использовании шаблона публикации и подписки существует третий компонент — брокер, брокер сообщений или служебная шина. Этот компонент известен как издателю, так и подписчику. Таким образом, при использовании шаблон публикации и подписки явным образом разделены благодаря упомянутой шине событий или брокеру сообщений.

### <a name="the-middleman-or-event-bus"></a>Шина событий, или посредник 

Как обеспечить анонимность между издателем и подписчиком? Самый простой способ — возложить весь обмен данными на посредника. Одним из таких посредников является шина событий.

Шина событий обычно состоит из двух частей:

-   Абстракция или интерфейс.

-   Одна или несколько реализаций.

На рис. 8-19 вы видите, что, с точки зрения приложения, шина событий — не что иное, как канал публикации и подписки. Реализовать этот асинхронный обмен данными можно различными способами. Он может иметь несколько реализаций, и вы можете переключаться между ними в зависимости от требований к среде (например, к рабочей среде по сравнению со средой разработки).

На рис. 8-20 показана абстракция шины событий с несколькими реализациями для различных технологий обмена сообщениями, таких как RabbitMQ, служебная шина Azure или другой брокер событий или брокер сообщений. 

![](./media/image21.png)

**Рис. 8-20.** Несколько реализаций шины событий

Тем не менее, как упоминалось ранее, использование собственных абстракций (интерфейса шины событий) разумно только в том случае, если вам нужны базовые функции шины событий, поддерживаемые вашими абстракциями. Если требуется более широкий набор функций, следует использовать API и абстракции, предоставляемые используемой коммерческой служебной шиной вместо собственных абстракций. 

### <a name="defining-an-event-bus-interface"></a>Определение интерфейса шины событий

Давайте начнем с реализации кода для интерфейса шины событий и возможных реализаций для целей исследования. Интерфейс должен быть универсальным и простым, как и следующий интерфейс.

```csharp
public interface IEventBus
{
    void Publish(IntegrationEvent @event);

    void Subscribe<T, TH>()
        where T : IntegrationEvent
        where TH : IIntegrationEventHandler<T>;

    void SubscribeDynamic<TH>(string eventName)
        where TH : IDynamicIntegrationEventHandler;

    void UnsubscribeDynamic<TH>(string eventName)
        where TH : IDynamicIntegrationEventHandler;

    void Unsubscribe<T, TH>()
        where TH : IIntegrationEventHandler<T>
        where T : IntegrationEvent;
}
```

Метод `Publish` достаточно прост. Шина событий передает полученное событие интеграции всем микрослужбам и даже внешним приложениям, которые подписаны на это событие. Этот метод используется микрослужбой, которая публикует событие.

Методы `Subscribe` (возможны различные реализации в зависимости от аргументов) используются микрослужбами, которые хотят получать события. У этого метода есть два аргумента. Первый — это событие интеграции, на которое он подписан (`IntegrationEvent`). Второй аргумент — обработчик события интеграции (или метод обратного вызова) с именем `IIntegrationEventHandler<T>`, который будет выполнен, когда микрослужба-получатель получит сообщение об этом событии интеграции.


>[!div class="step-by-step"]
[Назад](database-server-container.md)
[Вперед](rabbitmq-event-bus-development-test-environment.md)

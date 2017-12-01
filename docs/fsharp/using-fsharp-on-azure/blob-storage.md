---
title: "Начало работы с хранилищем больших двоичных объектов Azure с помощью F #"
description: "Хранить неструктурированные данные в облако с помощью хранилища больших двоичных объектов Azure."
keywords: "Visual f #, f #, функционального программирования, .NET, .NET Core, Azure"
author: sylvanc
ms.author: phcart
ms.date: 09/20/2016
ms.topic: article
ms.prod: .net
ms.technology: devlang-fsharp
ms.devlang: fsharp
ms.assetid: c5b74a4f-dcd1-4849-930c-904b6c8a04e1
ms.openlocfilehash: 92e26aff605d3bed89e388dd3616a2a9a3a96081
ms.sourcegitcommit: bd1ef61f4bb794b25383d3d72e71041a5ced172e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2017
---
# <a name="get-started-with-azure-blob-storage-using-f"></a>Начало работы с хранилищем больших двоичных объектов Azure с помощью F # #

Хранилище BLOB-объектов Azure — это служба, которая сохраняет неструктурированные данные в облаке в виде BLOB-объектов. В хранилище BLOB-объектов можно хранить любые типы текстовых или двоичных данных, таких как документы, файлы мультимедиа или установщики приложений. Хранилище BLOB-объектов также называют хранилищем объектов.

В этой статье показано, как выполнять типовые задачи с помощью хранилища больших двоичных объектов. Образцы записываются с помощью F # с помощью клиентской библиотеки хранилища Azure для .NET. Рассмотренные задачи включают как передача, список, загрузка и удаление больших двоичных объектов.

Общие сведения о хранилище больших двоичных объектов, в разделе [руководство по .NET для хранилища BLOB-объектов](/azure/storage/storage-dotnet-how-to-use-blobs).

## <a name="prerequisites"></a>Предварительные требования

Чтобы использовать это руководство, необходимо сначала [создать учетную запись хранилища Azure](/azure/storage/storage-create-storage-account). Необходимо также ключи доступа к хранилищу для этой учетной записи.

## <a name="create-an-f-script-and-start-f-interactive"></a>Создание скрипта F # и начала F #, интерактивный

Примеры в этой статье используется в F # приложения или скрипта F #. Чтобы создать скрипт F #, создайте файл с `.fsx` расширение, например `blobs.fsx`, в среде разработки F #.

Затем с помощью [диспетчера пакетов](package-management.md) например [Paket](https://fsprojects.github.io/Paket/) или [NuGet](https://www.nuget.org/) установка `WindowsAzure.Storage` и `Microsoft.WindowsAzure.ConfigurationManager` пакетов и ссылок `WindowsAzure.Storage.dll` и `Microsoft.WindowsAzure.Configuration.dll` в сценарий с помощью `#r` директивы.

### <a name="add-namespace-declarations"></a>Добавьте объявления пространств имен

Добавьте следующие `open` инструкции в начало `blobs.fsx` файла:

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L1-L5)]

### <a name="get-your-connection-string"></a>Получение строки подключения

В этом учебнике требуется строка подключения хранилища Azure. Дополнительные сведения о строках соединения см. в разделе [Настройка строки подключения хранилища](/azure/storage/storage-configure-connection-string).

В учебнике введите строку подключения в скрипте, следующим образом:

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L11-L11)]

Однако это **не рекомендуется** для реальных проектах. Ключ учетной записи хранилища аналогична корневой пароль для учетной записи хранилища. Всегда будьте внимательны защитить ключ учетной записи хранилища. Избегайте распространением их другим пользователям, жестко запрограммированные ее или сохранить в текстовый файл, доступный другим пользователям. Можно повторно создать ключ с помощью портала Azure, если вы считаете, что он был скомпрометирован.

Для реальных приложений для сохранения строки подключения хранилища рекомендуется в файле конфигурации. Чтобы получить строку подключения из файла конфигурации, это можно сделать:

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L13-L15)]

С помощью диспетчера конфигурации Azure является необязательным. Можно также использовать интерфейс API, например .NET Framework `ConfigurationManager` типа.

### <a name="parse-the-connection-string"></a>Синтаксический анализ строки соединения

Чтобы обработать строку подключения, используйте:

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L21-L22)]

Возвращает `CloudStorageAccount`.

### <a name="create-some-local-dummy-data"></a>Создать некоторые локальные фиктивные данные

Прежде чем начать, создайте некоторые фиктивный локальных данных в каталоге наш сценарий. Затем можно загрузить эти данные.

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L28-L30)]

### <a name="create-the-blob-service-client"></a>Создание клиента службы BLOB-объектов

`CloudBlobClient` Тип позволяет получить контейнеры и большие двоичные объекты, хранящиеся в хранилище больших двоичных объектов. Вот один из способов создания клиента службы.

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L36-L36)]

Теперь вы готовы написать код, который считывает и записывает данные в хранилище больших двоичных объектов.

## <a name="create-a-container"></a>Создание контейнера

В этом примере показано, как создать контейнер, если он еще не существует:

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L42-L46)]

По умолчанию новый контейнер является закрытым, это означает, что необходимо указать ключи доступа к хранилищу для загрузки больших двоичных объектов из этого контейнера. Если вы хотите сделать файлы в контейнере общедоступной, можно задать контейнер должен быть открытым, используя следующий код:

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L48-L49)]

Содержимое в Интернете могут видеть большие двоичные объекты в общедоступном контейнере, но можно изменить или удалить их только в том случае, если у вас есть ключ соответствующую учетную запись доступа или подписанный URL-адрес.

## <a name="upload-a-blob-into-a-container"></a>Отправить большой двоичный объект в контейнер

Хранилище больших двоичных объектов Azure поддерживает блочных и страничных больших двоичных объектов. В большинстве случаев блочного BLOB-объекта — это рекомендуемый тип для использования.

Чтобы отправить файл в большой двоичный объект блока, получить ссылку на контейнер и использовать его для получения ссылки блок больших двоичных объектов. Получив ссылку на большой двоичный объект можно передать любой поток данных в его путем вызова `UploadFromFile` метод. Эта операция создает большой двоичный объект, если он не существует, или ранее перезаписать его, если он существует.

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L55-L59)]

## <a name="list-the-blobs-in-a-container"></a>Перечисление BLOB-объектов в контейнере

Перечисление BLOB-объектов в контейнере, необходимо сначала получите ссылку на контейнер. Затем можно использовать контейнер `ListBlobs` метод для извлечения больших двоичных объектов и/или каталогов в ней. Для доступа к широкий набор свойств и методов для возвращенной `IListBlobItem`, его необходимо привести к типу `CloudBlockBlob`, `CloudPageBlob`, или `CloudBlobDirectory` объекта. Если тип неизвестен, чтобы определить, что приведение к типу можно использовать проверку типов. Следующий код показывает, как для извлечения и вывода каждого элемента в URI `mydata` контейнера:

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L67-L80)]

Вы также можете имя большие двоичные объекты с сведения о пути, в именах. Это создает структуру виртуального каталога, в которой можно упорядочивать и просматривать как и традиционные файловой системы. Обратите внимание, что структура каталогов виртуального только - только доступные ресурсы в хранилище больших двоичных объектов, контейнеры и большие двоичные объекты. Однако клиентская библиотека хранилища предоставляет `CloudBlobDirectory` объекта виртуального каталога и упростить процесс работы с большими двоичными объектами, которые упорядочены таким образом.

Например, рассмотрим следующий набор блочных больших двоичных объектов в контейнере с именем `photos`:

*photo1.jpg*
*2015/architecture/description.txt*
*2015/architecture/photo3.jpg*
*2015 или Архитектура/photo4.jpg*
*2016/architecture/photo5.jpg*
*2016/architecture/photo6.jpg* 
 *2016/architecture/description.txt*
*2016/photo7.jpg*

При вызове `ListBlobs` в контейнере (как в приведенном выше примере), возвращается иерархический список. Если он содержит оба `CloudBlobDirectory` и `CloudBlockBlob` объекты, представляющие каталоги и большие двоичные объекты в контейнере, соответственно, а затем полученный результат выглядит примерно так:

```console
Directory: https://<accountname>.blob.core.windows.net/photos/2015/
Directory: https://<accountname>.blob.core.windows.net/photos/2016/
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

При необходимости можно задать `UseFlatBlobListing` параметр `ListBlobs` метод `true`. В этом случае каждый большой двоичный объект в контейнере возвращается в виде `CloudBlockBlob` объекта. Вызов `ListBlobs` для возврата в плоском листинге выглядит следующим образом:

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L82-L89)]

Кроме того, в зависимости от текущее содержимое контейнера, результаты будут выглядеть следующим образом:

```console
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2015/architecture/description.txt
Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2015/architecture/photo3.jpg
Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2015/architecture/photo4.jpg
Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2016/architecture/description.txt
Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2016/architecture/photo5.jpg
Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2016/architecture/photo6.jpg
Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2016/photo7.jpg
Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg
```

## <a name="download-blobs"></a>Загрузка больших двоичных объектов

Чтобы загрузить больших двоичных объектов, сначала получить ссылку на большой двоичный объект, а затем вызвать `DownloadToStream` метод. В следующем примере используется `DownloadToStream` способ передачи содержимого большого двоичного объекта в объект потока, могут затем сохраняться в локальный файл.

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L95-L101)]

Можно также использовать `DownloadToStream` метод для загрузки содержимого большого двоичного объекта в виде текстовой строки.

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L103-L106)]

## <a name="delete-blobs"></a>Удаление больших двоичных объектов

Чтобы удалить большой двоичный объект, сначала получить ссылку на большой двоичный объект и затем вызвать `Delete` метода.

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L112-L116)]

## <a name="list-blobs-in-pages-asynchronously"></a>Перечисление больших двоичных объектов, на страницах асинхронно

Если вы хотите управлять количество результатов, возвращаемых в одной операцией получения листинга списка большое количество больших двоичных объектов, можно перечислить большие двоичные объекты в страницы результатов. В этом примере показано, как асинхронно, возвращают результаты в страницах, чтобы выполнение не блокируется во время ожидания для возвращения большого набора результатов.

В этом примере показано плоский перечисления больших двоичных объектов, но можно также выполнить, задав иерархический список `useFlatBlobListing` параметр `ListBlobsSegmentedAsync` метод `false`.

Пример определяет асинхронный метод, с помощью `async` блока. ``let!`` Ключевое слово приостанавливает выполнение образец метода до завершения задачи в список.

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L122-L160)]

Эта процедура асинхронной теперь можно использовать следующим образом. Сначала необходимо отправить некоторые фиктивные данные (с помощью локального файла, созданного ранее в этом учебнике).

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L162-L166)]

Теперь вызовите процедуру. Вы используете ``Async.RunSynchronously`` для принудительного выполнения асинхронной операции.

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L168-L168)]

## <a name="writing-to-an-append-blob"></a>Запись в добавочный BLOB-объект

Добавочный большой двоичный объект оптимизирован для операций добавления, например ведение журнала. Как большой двоичный объект блока добавочный BLOB-объект состоит из блоков, но при добавлении нового блока добавочный большой двоичный объект всегда добавляются в конец большого двоичного объекта. Нельзя обновить или удалить существующий блок в добавочный большой двоичный объект. Идентификаторы блокировок добавочный большой двоичный объект не отображаются, как и для большого двоичного объекта.

Каждый блок в добавочный большой двоичный объект может быть разный размер не более 4 МБ и добавочный большой двоичный объект может содержать не более 50 000 блоков. Максимальный размер добавочный большой двоичный объект является таким образом немного более 195 ГБ (4 МБ X 50 000 блоков).

Следующий пример создает новый добавочный большой двоичный объект и добавляет некоторые данные, имитируя операция простого ведения журнала.

[!code-fsharp[BlobStorage](../../../samples/snippets/fsharp/azure/blob-storage.fsx#L174-L203)]

В разделе [основные сведения о блочных, страничные большие двоичные объекты и добавьте большие двоичные объекты](https://msdn.microsoft.com/library/azure/ee691964.aspx) Дополнительные сведения о различиях между три типа больших двоичных объектов.

## <a name="concurrent-access"></a>Параллельный доступ

Для поддержки параллельного доступа в большой двоичный объект из нескольких клиентов или несколько экземпляров процесса, можно использовать **ETags** или **аренды**.

* **ETag** -позволяет обнаружить, что большой двоичный объект или контейнер был изменен другим процессом

* **Аренда** -предоставляет способ получения монопольного, обновляемым, запись или удаление доступа к BLOB-объекта в течение заданного времени

Дополнительные сведения см. в разделе [управление параллелизмом в хранилище Microsoft Azure](https://azure.microsoft.com/blog/managing-concurrency-in-microsoft-azure-storage-2/).

## <a name="naming-containers"></a>Контейнеры именования

Каждый BLOB-объектов в хранилище Azure должен находиться в контейнере. Контейнер составляет часть имени большого двоичного объекта. Например `mydata` — имя контейнера в этих blob образец идентификаторы URI:

    https://storagesample.blob.core.windows.net/mydata/blob1.txt
    https://storagesample.blob.core.windows.net/mydata/photos/myphoto.jpg

Имя контейнера должно быть допустимым именем DNS, удовлетворяющих следующим правилам:

1. Имена контейнеров должны начинаться с буквы или цифры и может содержать только буквы, цифры и тире (-).
1. Каждому символу тире (-) должен быть немедленно и стоять буква или цифра; последовательные дефисы не допускаются в именах контейнеров.
1. Все буквы в имени контейнера должны быть строчными.
1. Длина имени контейнера должна составлять от 3 до 63 символов.

Обратите внимание, что имя контейнера всегда должен быть нижнего регистра. Если включить буквой верхнего регистра в имени контейнера или иным образом нарушать правила именования контейнера, может появиться ошибку 400 (неправильный запрос).

## <a name="managing-security-for-blobs"></a>Управление безопасностью для больших двоичных объектов

По умолчанию хранилища Azure защищает ваши данные, ограничивая доступ к учетной записи владельца, являющегося владеющим ключей доступа учетных записей. Если вам требуется общий доступ к данным BLOB-объектов в вашей учетной записи хранилища, важно сделать без риска для безопасности ключей доступа к учетной записи. Кроме того можно зашифровать данные большого двоичного объекта, чтобы обеспечить его безопасность, перейдя по сети и в хранилище Azure.

### <a name="controlling-access-to-blob-data"></a>Управление доступом к данным BLOB-объект

По умолчанию данные blob в учетной записи доступен только владельцу учетной записи хранилища. Проверка подлинности запросов в службу хранилища больших двоичных объектов требуется ключ доступа учетной записи по умолчанию. Тем не менее может потребоваться сделать определенные данные больших двоичных объектов доступным другим пользователям.

Дополнительные сведения о том, как управлять доступом к хранилищу больших двоичных объектов см. в разделе [руководство по .NET для раздела хранилища больших двоичных объектов для управления доступом](/azure/storage/storage-dotnet-how-to-use-blobs#controlling-access-to-blob-data).


### <a name="encrypting-blob-data"></a>Шифрование данных больших двоичных объектов

Хранилище Azure поддерживает шифрование данных больших двоичных объектов как на клиенте, так и на сервере.

Дополнительные сведения о шифровании данных больших двоичных объектов см. в разделе [руководство по .NET для раздела хранилища BLOB-объектов на шифрование](/azure/storage/storage-dotnet-how-to-use-blobs#encrypting-blob-data).

## <a name="next-steps"></a>Следующие шаги

Теперь, когда вы узнали основы хранилища больших двоичных объектов, приведены по следующим ссылкам, чтобы получить дополнительные сведения.

### <a name="tools"></a>Инструменты
- [F # AzureStorageTypeProvider](http://fsprojects.github.io/AzureStorageTypeProvider/) поставщик типа F #, который можно использовать для изучения средств Blob, таблицы и очереди хранилища Azure и легко применять операции CRUD над ними.
- [FSharp.Azure.Storage](https://github.com/fsprojects/FSharp.Azure.Storage) API F # для использования службы хранилища таблиц Microsoft Azure
- [Обозреватель хранилища Microsoft Azure (MASE)](/azure/vs-azure-tools-storage-manage-with-storage-explorer) является бесплатно, отдельное приложение от Майкрософт, которая позволяет визуально работать с данными в хранилище Azure в Windows, Linux и OS X.

### <a name="blob-storage-reference"></a>Справочник по хранилища больших двоичных объектов

- [Клиентская библиотека хранилища для Справочник по .NET](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [Справочник по REST API](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="related-guides"></a>Связанные направляющие

- [Приступая к работе с хранилищем BLOB-объектов Azure в C#](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)
- [Перенесите данные с помощью программы командной строки AzCopy](/azure/storage/storage-use-azcopy)
- [Настройка строк подключения](http://msdn.microsoft.com/library/azure/ee758697.aspx)
- [Блог группы разработчиков хранилища Azure](http://blogs.msdn.com/b/windowsazurestorage/)
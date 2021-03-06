---
title: Типы XML и ADO.NET в контрактах данных
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c2ce8461-3c15-4c41-8c81-1cb78f5b59a6
ms.openlocfilehash: ae21174d19ad69f87165427cf5a0bfd29ac872db
ms.sourcegitcommit: 3d5d33f384eeba41b2dff79d096f47ccc8d8f03d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/04/2018
ms.locfileid: "33509111"
---
# <a name="xml-and-adonet-types-in-data-contracts"></a>Типы XML и ADO.NET в контрактах данных
Модель контракта данных Windows Communication Foundation (WCF) поддерживает определенные типы, которые непосредственно представляют XML. При сериализации данных типов в XML сериализатор сохраняет содержимое таких типов XML без какой-либо дополнительной обработки. Поддерживаемые типы: <xref:System.Xml.XmlElement>, массивы <xref:System.Xml.XmlNode> (но не сами типы `XmlNode`), а также типы, реализующие <xref:System.Xml.Serialization.IXmlSerializable>. Типы <xref:System.Data.DataSet> и <xref:System.Data.DataTable>, а также типизированные наборы данных, чаще всего используются в программировании баз данных. Данные типы реализуют интерфейс `IXmlSerializable`, и, следовательно, сериализуются в модели контракта данных. Некоторые специальные рекомендации, относящиеся к данным типам, приведены в конце данного раздела.  
  
## <a name="xml-types"></a>Типы XML  
  
### <a name="xml-element"></a>Элемент XML  
 Тип `XmlElement` сериализуется с помощью содержимого XML. Например, с помощью следующего типа.  
  
 [!code-csharp[DataContractAttribute#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#4)]
 [!code-vb[DataContractAttribute#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#4)]  
  
 Данный тип сериализуется в XML следующим образом:  
  
```xml  
<MyDataContract xmlns="http://schemas.contoso.com">  
    <myDataMember>  
        <myElement xmlns="" myAttribute="myValue">  
            myContents  
        </myElement>  
    </myDataMember>  
</MyDataContract>  
```  
  
 Обратите внимание, что элемент данных программы-оболочки `<myDataMember>` все еще присутствует. Удалить данный элемент в модели контракта данных невозможно. Сериализаторы, обрабатывающие данную модель (<xref:System.Runtime.Serialization.DataContractSerializer> и <xref:System.Runtime.Serialization.NetDataContractSerializer>), могут выдать специальные атрибуты в данный элемент программы-оболочки. Данные атрибуты включают атрибут "nil" экземпляра схемы XML (позволяющий `XmlElement` принимать значение `null`) и атрибут "type" (позволяющий использовать `XmlElement` полиморфно). Кроме того, относящихся к WCF следующие атрибуты XML: «Id», «Ref», «Type» и «Assembly». Данные атрибуты могут быть выданы для поддержки использования элемента `XmlElement` в режиме сохранения графов объектов или при обработке сериализатором <xref:System.Runtime.Serialization.NetDataContractSerializer>. (Дополнительные сведения о режим сохранения графа объекта см. в разделе [сериализации и десериализации](../../../../docs/framework/wcf/feature-details/serialization-and-deserialization.md).)  
  
 Массивы коллекций `XmlElement` разрешены и обрабатываются так, как и любой другой массив или коллекция. Есть элемент программы-оболочки для всей коллекции и отдельный элемент программы-оболочки (подобный `<myDataMember>` из предыдущего примера) для каждого элемента `XmlElement` в массиве.  
  
 При десериализации элемент `XmlElement` создается с помощью десериализатора из входящего кода XML. Допустимый родительский <xref:System.Xml.XmlDocument> предоставляется десериализатором.  
  
 Убедитесь, что фрагмент XML, из которого десериализуется элемент `XmlElement`, определяет все префиксы, которые он использует, и не применяет какие-либо определения префикса из предков. Такая проблема возникает только при использовании `DataContractSerializer` для доступа к XML из отличного (не `DataContractSerializer`) источника.  
  
 При использовании с `DataContractSerializer`, `XmlElement` может быть назначен полиморфно, но только элементу данных типа <xref:System.Object>. Даже несмотря на то, что он реализует <xref:System.Collections.IEnumerable>, `XmlElement` не может быть использован как тип коллекции и не может быть назначен элементу данных <xref:System.Collections.IEnumerable>. Как и в случае со всеми полиморфными назначениями, `DataContractSerializer` передает имя контракта данных в получаемый код XML – в данном случае это «XmlElement» в "http://schemas.datacontract.org/2004/07/System.Xml" пространства имен.  
  
 С `NetDataContractSerializer` поддерживается любое допустимое полиморфное назначение `XmlElement` (в `Object` или `IEnumerable`).  
  
 Не пытайтесь использовать ни один из сериализаторов для типов, унаследованных от `XmlElement`, независимо от того, полиморфно они назначены или нет.  
  
### <a name="array-of-xmlnode"></a>Массив типа XmlNode  
 Использование массивов <xref:System.Xml.XmlNode> очень похоже на использование `XmlElement`. Использование массивов `XmlNode` обеспечивает большую гибкость, чем использование `XmlElement`. Можно записать несколько элементов внутри элемента программы-оболочки элемента данных. Кроме того, можно вставить содержимое, которое отличается от элементов внутри элемента программы-оболочки элемента данных, например комментарии XML. И наконец, в элемент программы-оболочки элемента данных можно поместить атрибуты. Все это можно сделать путем указания массива `XmlNode` со специальными унаследованными классами `XmlNode`, такими как <xref:System.Xml.XmlAttribute>, `XmlElement` или <xref:System.Xml.XmlComment>. Например, с помощью следующего типа.  
  
 [!code-csharp[DataContractAttribute#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/datacontractattribute/cs/overview.cs#5)]
 [!code-vb[DataContractAttribute#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/datacontractattribute/vb/overview.vb#5)]  
  
 При сериализации получаемый код XML подобен приведенному ниже коду.  
  
```xml  
<MyDataContract xmlns="http://schemas.contoso.com">  
  <myDataMember myAttribute="myValue">  
     <!--myComment-->  
     <myElement xmlns="" myAttribute="myValue">  
 myContents  
     </myElement>  
     <myElement xmlns="" myAttribute="myValue">  
       myContents  
     </myElement>  
  </myDataMember>  
</MyDataContract>  
```  
  
 Обратите внимание, что элемент программы-оболочки элемента данных `<myDataMember>` включает атрибут, комментарий и два элемента. Это четыре сериализированные экземпляра `XmlNode`.  
  
 Массив `XmlNode`, который получается в недопустимом XML-коде, не может быть сериализован. Например, массив из двух экземпляров `XmlNode`, из которых один - `XmlElement`, а другой - <xref:System.Xml.XmlAttribute>, является недействительным, поскольку данная последовательность не соответствует какому-либо допустимому экземпляру XML (в нем нет места, чтобы вложить атрибут).  
  
 При десериализации массива `XmlNode` узлы создаются и заполняются информацией из входящего XML-кода. Допустимый родительский <xref:System.Xml.XmlDocument> предоставляется десериализатором. Все узлы десериализуются, включая все атрибуты в элемент программы-оболочки элемента данных, но за исключением атрибутов, размещенных в них сериализаторами WCF (например, атрибуты, используемые для указания на полиморфное назначение). Примечание об определении всех префиксов пространства имен во фрагменте XML-кода применяется к десериализации массивов `XmlNode` точно так же, как при десериализации `XmlElement`.  
  
 При использовании сериализаторов с включенным режимом сохранения графов объектов равенство объектов сохраняется только на уровне массивов `XmlNode`, а не отдельных экземпляров `XmlNode`.  
  
 Не пытайтесь сериализовать массив `XmlNode`, где один или более узлов установлены в значение `null`. Значение `null` разрешено применять ко всему элементу массива, а не к отдельному `XmlNode`, содержащемуся в массиве. Если весь элемент данных массива пуст, элемент данных программы-оболочки содержит специальный атрибут, указывающий, что элемент данных массива пуст. При десериализации весь элемент данных массива также становится пустым.  
  
 Специально обрабатываются сериализатором только обычные массивы `XmlNode`. Элементы данных, объявленные как другие типы коллекций, содержащие `XmlNode`, или элементы данных, объявленные как массивы типов, унаследованных от `XmlNode`, специально не обрабатываются. Это означает, что обычно они не сериализуются, кроме случаев, когда они также соответствуют одному из других критериев для сериализации.  
  
 Массивы или коллекции массивов `XmlNode` разрешены. Есть элемент программы-оболочки для всей коллекции и отдельный элемент программы-оболочки (подобный `<myDataMember>` из предыдущего примера) для каждого массива `XmlNode` во внешнем массиве или коллекции.  
  
 Указание элемента данных типа <xref:System.Array> класса `Object` или `Array` интерфейса `IEnumerable` с экземплярами `XmlNode` не приводит к обработке элемента данных как экземпляров `Array` класса `XmlNode`. Каждый элемент данных сериализуется отдельно.  
  
 При использовании с `DataContractSerializer` массивы `XmlNode` могут быть назначены полиморфно, но только элементу данных типа `Object`. Даже несмотря на реализацию интерфейса `IEnumerable` массив `XmlNode` не может быть использован как тип коллекции и не может быть назначен элементу данных `IEnumerable`. Как и в случае со всеми полиморфными назначениями, `DataContractSerializer` передает имя контракта данных в получаемый код XML – в данном случае это «ArrayOfXmlNode» в "http://schemas.datacontract.org/2004/07/System.Xml" пространства имен. При использовании с `NetDataContractSerializer`, любое действительное назначение `XmlNode` массив поддерживается.  
  
### <a name="schema-considerations"></a>Замечания по схемам  
 Дополнительные сведения о сопоставлении схемы XML типов см. в разделе [Справочник по схеме контрактов данных](../../../../docs/framework/wcf/feature-details/data-contract-schema-reference.md). В данном разделе приводится обзор важных моментов.  
  
 Элемент данных типа `XmlElement` с элементом, определенным с помощью следующего анонимного типа.  
  
```xml  
<xsd:complexType>  
   <xsd:sequence>  
      <xsd:any minOccurs="0" processContents="lax" />  
   </xsd:sequence>  
</xsd:complexType>  
```  
  
 Элемент данных типа массива `XmlNode` сопоставлен с элементом, определенным с помощью следующего анонимного типа.  
  
```xml  
<xsd:complexType mixed="true">  
   <xsd:sequence>  
      <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax" />  
   </xsd:sequence>  
   <xsd:anyAttribute/>  
</xsd:complexType>  
```  
  
## <a name="types-implementing-the-ixmlserializable-interface"></a>Типы, реализующие интерфейс IXmlSerializable  
 Типы, реализующие интерфейс `IXmlSerializable`, полностью поддерживаются сериализатором `DataContractSerializer`. К данным типам всегда нужно применять атрибут <xref:System.Xml.Serialization.XmlSchemaProviderAttribute>, необходимый для управления их схемой.  
  
 Существует три разновидности типов, реализующих интерфейс `IXmlSerializable`: типы, представляющие производное содержимое; типы, представляющие одиночный элемент; устаревшие типы <xref:System.Data.DataSet>.  
  
-   Типы содержимого используют метод поставщика схемы, заданный атрибутом `XmlSchemaProviderAttribute`. Метод не возвращает значение `null`, и свойство <xref:System.Xml.Serialization.XmlSchemaProviderAttribute.IsAny%2A> атрибута остается в значении по умолчанию `false`. Это наиболее распространенное использование типов `IXmlSerializable`.  
  
-   Типы элемента используется в том случае, когда тип `IXmlSerializable` должен управлять собственным именем корневого элемента. Чтобы отметить тип как тип элемента, свойство <xref:System.Xml.Serialization.XmlSchemaProviderAttribute.IsAny%2A> атрибута <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> устанавливается в значение `true`, или из метода поставщика схемы возвращается значение null. Наличие метода поставщика схемы является необязательным для типов элементов – вместо имени метода в `XmlSchemaProviderAttribute` можно указать null. Однако, если `IsAny` имеет значение `true` и указан метод поставщика схемы, метод должен возвратить значение null.  
  
-   Устаревшие типы <xref:System.Data.DataSet> являются типами `IXmlSerializable`, не отмеченными атрибутом `XmlSchemaProviderAttribute`. Вместо этого они используют для создания схемы метод <xref:System.Xml.Serialization.IXmlSerializable.GetSchema%2A>. Данный шаблон используется для типа `DataSet`, и его типизированный набор данных наследует класс в более ранних версиях .NET Framework; в настоящее время он устарел и поддерживается только из соображений совместимости с более ранними версиями. Не используйте данный шаблон и всегда применяйте атрибут `XmlSchemaProviderAttribute` к своим типам `IXmlSerializable`.  
  
### <a name="ixmlserializable-content-types"></a>Типы содержимого IXmlSerializable  
 При сериализации элемента данных соответствующего типа, который реализует `IXmlSerializable` и относится к определенному ранее типу содержимого, сериализатор записывает элемент программы-оболочки для элемента данных и передает управление методу <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A>. Реализация <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A> может записать любой XML, включая добавление атрибутов в элемент программы-оболочки. После выполнения `WriteXml` сериализатор закрывает элемент.  
  
 При десериализации элемента данных соответствующего типа, который реализует `IXmlSerializable` и относится к определенному ранее типу содержимого, десериализатор устанавливает средство чтения XML на элемент программы-оболочки для элемента данных и передает управление методу <xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A>. Метод должен прочесть весь элемент, включая открывающий и закрывающий теги. Убедитесь, что код `ReadXml` обрабатывает случай, когда элемент пуст. Кроме того, реализация `ReadXml` не должна использовать элемент программы-оболочки, именованный особым образом. Имя, выбранное сериализатором, может изменяться.  
  
 Разрешается полиморфно присваивать типы содержимого `IXmlSerializable`, например, элементам данных типа <xref:System.Object>. Кроме того, для экземпляров типа разрешено значение null. И наконец, можно использовать типы `IXmlSerializable` с включенным режимом сохранения графов объектов и с <xref:System.Runtime.Serialization.NetDataContractSerializer>. Все эти функции требуют сериализатор WCF, вложил некоторые атрибуты в элемент программы-оболочки («nil» и «type» в имен экземпляра схемы XML и «Id», «Ref», «Type» и «Assembly» в пространстве имен определенного WCF).  
  
#### <a name="attributes-to-ignore-when-implementing-readxml"></a>Атрибуты, игнорируемые при реализации ReadXml  
 Перед передачей управления коду `ReadXml` десериализатор проверяет XML-элемент, обнаруживает данные специальные атрибуты XML и работает с ними. Например, если "nil" имеет значение `true`, десериализуется значение null, и `ReadXml` не вызывается. Если обнаружен полиморфизм, содержимое десериализуется так, как если бы это был другой тип. Вызывается реализация `ReadXml` полиморфно назначенного типа. В любом случае, реализация `ReadXml` игнорирует данные специальные атрибуты, поскольку они обрабатываются десериализатором.  
  
### <a name="schema-considerations-for-ixmlserializable-content-types"></a>Замечания по схемам для типов содержимого IXmlSerializable  
 При экспорте схемы с типом содержимого `IXmlSerializable` вызывается метод поставщика схемы. <xref:System.Xml.Schema.XmlSchemaSet> передается в метод поставщика схемы. Метод может добавить любую допустимую схему в набор схем. Набор схем содержит схему, которая уже была известна на момент экспорта схемы. Если метод поставщика схем должен добавить элемент в набор схем, он должен определить, имеется ли в наборе <xref:System.Xml.Schema.XmlSchema> с соответствующим пространством имен. Если это так, метод поставщика схемы должен добавить новый элемент в существующую схему `XmlSchema`. В противном случае, метод создает новый экземпляр `XmlSchema`. Это важно, если используются массивы типов `IXmlSerializable`. Например, если есть тип `IXmlSerializable`, который экспортируется как тип "A" в пространстве имен "Б", возможно, что к моменту вызова метода поставщика схем, набор схем уже содержит схему для "Б" для удержания типа "ArrayOfA".  
  
 Кроме добавления типов в <xref:System.Xml.Schema.XmlSchemaSet>, метод поставщика схемы для типов содержимого должен возвратить ненулевое значение. Метод может возвратить <xref:System.Xml.XmlQualifiedName>, указывающий имя типа схемы, которая будет использоваться для заданного типа `IXmlSerializable`. Полное имя также служит именем контракта данных и пространством имен для типа. Не разрешается возвращать тип, не существующий в наборе схем, немедленно при возврате метода поставщика. Однако предполагается, что к моменту экспорта всех типов (метод <xref:System.Runtime.Serialization.XsdDataContractExporter.Export%2A> вызывается для всех соответствующих типов <xref:System.Runtime.Serialization.XsdDataContractExporter> и выполняется доступ к свойству <xref:System.Runtime.Serialization.XsdDataContractExporter.Schemas%2A>) тип существует в наборе схем. Доступ к свойству `Schemas` до того, как были выполнены все соответствующие вызовы `Export`, может привести к созданию исключения <xref:System.Xml.Schema.XmlSchemaException>. Дополнительные сведения о процессе экспорта см. в разделе [Экспорт схем из классов](../../../../docs/framework/wcf/feature-details/exporting-schemas-from-classes.md).  
  
 Метод поставщика схемы также может возвратить тип <xref:System.Xml.Schema.XmlSchemaType> для использования. Тип может быть или не быть анонимным. Если тип анонимный, схема для типа `IXmlSerializable` экспортируется как анонимный тип при каждом использовании типа `IXmlSerializable` в качестве элемента данных. Тип `IXmlSerializable` все еще имеет контракт данных и пространство имен. (Это определить, как описано в [имена контрактов данных](../../../../docs/framework/wcf/feature-details/data-contract-names.md) за исключением того, что <xref:System.Runtime.Serialization.DataContractAttribute> атрибут не может использоваться для пользовательского имени.) Если тип не анонимный, он должен быть одним из типов в наборе схем `XmlSchemaSet`. Данный случай эквивалентен возврату `XmlQualifiedName` типа.  
  
 Кроме того, для типа экспортируется глобальное объявление элемента. Если тип не имеет примененного к нему атрибута <xref:System.Xml.Serialization.XmlRootAttribute>, элемент имеет те же имя и пространство имен, что контракт данных, и его свойство "nillable" имеет значение "true". Единственным исключением является схема пространства имен («http://www.w3.org/2001/XMLSchema») — Если контракт данных типа в этом пространстве имен, соответствующий глобальный элемент находится в пустом пространстве имен, поскольку запрещена для добавления новых элементов к пространству имен схемы. Если тип имеет применяемый к нему атрибут `XmlRootAttribute`, глобальное объявление элемента экспортируется с помощью свойств <xref:System.Xml.Serialization.XmlRootAttribute.ElementName%2A>, <xref:System.Xml.Serialization.XmlRootAttribute.Namespace%2A> и <xref:System.Xml.Serialization.XmlRootAttribute.IsNullable%2A>. Значениями по умолчанию при применении атрибута `XmlRootAttribute` являются имя контракта данных, пустое пространство имен и параметр "nillable" в значении "true".  
  
 Те же правила объявления глобального элемента применяются и к устаревшим типам наборов данных. Важно отметить, что `XmlRootAttribute` не может переопределить объявления глобальных элементов, добавленных с помощью пользовательского кода или добавленных в набор схем `XmlSchemaSet` с помощью метода поставщика схем или посредством метода `GetSchema` для устаревших типов наборов данных.  
  
### <a name="ixmlserializable-element-types"></a>Типы элемента IXmlSerializable  
 Типы элементов `IXmlSerializable` либо имеют свойство `IsAny`, которому присвоено значение `true`, либо их метод поставщика схем возвращает значение `null`.  
  
 Сериализация и десериализация типа элемента очень похожа на сериализацию и десериализацию типа содержимого. Однако есть некоторые важные отличия.  
  
-   Как правило, реализация `WriteXml` записывает только один элемент (который, конечно, может содержать несколько дочерних элементов). Она не должна записывать атрибуты вне данного одиночного элемента, несколько родственных элементов или смешанное содержимое. Элемент может быть пустым.  
  
-   Реализация `ReadXml` не должна прочитывать элемент программы-оболочки. Как правило, реализация прочитывает один элемент, создаваемый методом `WriteXml`.  
  
-   При регулярной сериализации типа элемента (например, как элемента данных в контракте данных) сериализатор, как и в случае с типами содержимого, выводит элемент программы-оболочки до вызова метода `WriteXml`. Однако при сериализации типа элемента на верхнем уровне сериализатор обычно не выводит элемент программы-оболочки в окружение элемента, который выводится методом `WriteXml`, кроме случая, когда корневое имя и пространство имен были явно заданы при конструировании сериализатора в конструкторах `DataContractSerializer` или `NetDataContractSerializer`. Дополнительные сведения см. в разделе [сериализации и десериализации](../../../../docs/framework/wcf/feature-details/serialization-and-deserialization.md).  
  
-   При сериализации типа элемента на верхнем уровне без указания корневого имени и пространства имен во время создания <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteStartObject%2A> и <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteEndObject%2A> обычно не выполняют никаких операций, а <xref:System.Runtime.Serialization.XmlObjectSerializer.WriteObjectContent%2A> вызывает `WriteXml`. В данном режиме сериализуемый объект не может иметь значение null и не может быть назначен полиморфно. Кроме того, не может быть включено сохранение графов объектов и не может использоваться `NetDataContractSerializer`.  
  
-   При десериализации типа элемента на верхнем уровне без указания корневого имени и пространства имен во время построения <xref:System.Runtime.Serialization.XmlObjectSerializer.IsStartObject%2A> возвращает значение `true`, если не может найти начало хотя бы одного из элементов. <xref:System.Runtime.Serialization.XmlObjectSerializer.ReadObject%2A> с параметром `verifyObjectName`, установленным в значение `true`, работает таким же образом, как и `IsStartObject` перед фактическим считыванием объекта. Затем `ReadObject` передает управление методу `ReadXml`.  
  
 Схема, экспортированная для типов элементов, аналогична схеме для типа `XmlElement`, как описано в предыдущем разделе, за исключением того, что метод поставщика схемы может добавлять дополнительную схему в <xref:System.Xml.Schema.XmlSchemaSet> как типы содержимого. Использование атрибута `XmlRootAttribute` с типами элемента не разрешено, и для данных типов никогда не выдаются глобальные объявления элемента.  
  
### <a name="differences-from-the-xmlserializer"></a>Отличия от XmlSerializer  
 Сериализатор `IXmlSerializable` также понимает интерфейс `XmlSchemaProviderAttribute` и атрибуты `XmlRootAttribute` и <xref:System.Xml.Serialization.XmlSerializer>. Однако есть некоторые отличия в том, как данные атрибуты обрабатываются в модели контракта данных. Сводка важнейших отличий представлена ниже.  
  
-   Метод поставщика схемы должен быть открытым для использования в `XmlSerializer`, но не должен быть открытым для использования в модели контракта данных.  
  
-   Метод поставщика схемы вызывается, если свойству `IsAny` присвоено значение true в модели контракта данных, но не с `XmlSerializer`.  
  
-   Если атрибут `XmlRootAttribute` не присутствует в содержимом или устаревших типах набора данных, сериализатор `XmlSerializer` экспортирует глобальное объявление элемента в пустое пространство имени. В модели контракта данных используемым пространством имен обычно является пространство имен контракта данных, как было описано ранее.  
  
 Помните об этих отличиях при создании типов, которые используются с обеими технологиями сериализации.  
  
### <a name="importing-ixmlserializable-schema"></a>Импорт схемы IXmlSerializable  
 При импорте схемы, созданной из типов `IXmlSerializable`, существует несколько возможностей.  
  
-   Созданная схема может быть действительной схемой контракта данных, как описано в [Справочник по схеме контрактов данных](../../../../docs/framework/wcf/feature-details/data-contract-schema-reference.md). В таком случае, схема может быть импортирована обычным образом, и создаются обычные типы контракта данных.  
  
-   Созданная схема может не быть действительной схемой контракта данных. Например, ваш собственный метод поставщика схемы может создать схему, которая включает атрибуты XML, не поддерживаемые в модели контракта данных. В данном случае можно импортировать схему как типы `IXmlSerializable`. Этот режим импорта по умолчанию не установлен, но можно легко включить — например, с `/importXmlTypes` используйте параметр командной строки [ServiceModel Metadata Utility Tool (Svcutil.exe)](../../../../docs/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe.md). Это подробно описан в [Импорт схемы для создания классов](../../../../docs/framework/wcf/feature-details/importing-schema-to-generate-classes.md). Обратите внимание, что работать нужно непосредственно с XML собственных экземпляров типа. Кроме того, следует принимать во внимание другую технологию сериализации, поддерживающую широкий диапазон схем - см. раздел, посвященный использованию `XmlSerializer`.  
  
-   Возможно, вам понадобится повторно использовать существующие типы `IXmlSerializable` в прокси, вместо того чтобы создавать новые. В таком случае для указания типа для повторного использования может использоваться возможность ссылочных типов, описанная в разделе «Импорт схемы для создания типов». Это соответствует использованию `/reference` переключиться на svcutil.exe, указывающего на сборку, содержащую типы для повторного использования.  
  
## <a name="representing-arbitrary-xml-in-data-contracts"></a>Представление произвольного XML в контракте данных  
 Типы `XmlElement`, массив `XmlNode` и `IXmlSerializable` позволяют вводить производный XML в модель контракта данных. `DataContractSerializer` и `NetDataContractSerializer`, не нарушая процесса, передают данное содержимое XML в используемое средство записи XML. Однако средства записи XML могут принудительно задать некоторые ограничения на записываемый ими XML. В частности, далее приводится несколько важных примеров.  
  
-   Средства записи XML обычно не разрешают объявления XML-документа (например, \<? версия xml = "1.0"? >) в середине записи другого документа. Нельзя взять полный XML-документ и сериализовать его как `Array` элемента данных `XmlNode`. Чтобы сделать это, нужно либо удалить объявление документа, либо использовать для его представления собственную схему кодирования.  
  
-   Все средства записи XML, поставляемых вместе с WCF отклоняют инструкции по обработке XML (\<? … ? >) и определения типа документа (\<! … >), так как они не разрешены в сообщениях SOAP. И, опять же, чтобы обойти данное ограничение, можно использовать собственный механизм кодирования. Если нужно включить их в готовый XML-код, можно написать пользовательский кодировщик, использующий поддерживающие их средства записи XML.  
  
-   При реализации метода `WriteXml` избегайте вызова метода в средстве записи XML <xref:System.Xml.XmlWriter.WriteRaw%2A>. WCF использует различные кодировки XML (включая двоичные), очень сложно или невозможно использовать `WriteRaw` таким образом, чтобы результат можно было использовать в любой кодировке.  
  
-   При реализации `WriteXml`, избегайте использования <xref:System.Xml.XmlWriter.WriteEntityRef%2A> и <xref:System.Xml.XmlWriter.WriteNmToken%2A> методы, которые не поддерживаются средствами записи XML, предоставляемыми с WCF.  
  
## <a name="using-dataset-typed-dataset-and-datatable"></a>Использование наборов данных, типизированных наборов данных и таблиц данных  
 Использование данных типов полностью поддерживается в модели контракта данных. При использовании данных типов необходимо учитывать следующие моменты.  
  
-   Схема для этих типов (особенно <xref:System.Data.DataSet> и его наследуемые типизированные классы) не может взаимодействовать с некоторые платформы не WCF или может привести к снижению удобства использования при работе с данными платформами. Кроме того, использование типа `DataSet` может отрицательно влиять на производительность. Наконец, это усложняет создание новой версии вашего приложения в будущем. Рассмотрите вариант использования в ваших контрактах явным образом определенных типов контракта данных вместо типов `DataSet`.  
  
-   При импорте схем `DataSet` или `DataTable` важно сделать ссылку на эти типы. С помощью средства командной строки Svcutil.exe это можно сделать, передав имя сборки System.Data.dll `/reference` переключения. Если импортируется схема типизированного набора данных, необходимо создать ссылку на тип типизированного набора данных. С помощью Svcutil.exe, передать расположение сборки типизированного набора данных для `/reference` переключения. Дополнительные сведения о ссылках на типы см. в разделе [Импорт схемы для создания классов](../../../../docs/framework/wcf/feature-details/importing-schema-to-generate-classes.md).  
  
 Поддержка типизированных наборов данных в контракте данных ограничена. Типизированные наборы данных могут быть сериализованы и десериализованы, а также могут экспортировать свою схему. Однако импорт схемы контракта данных не способен создать новые типы типизированных наборов данных, поскольку в импорте могут повторно использоваться только уже существующие наборы данных. Можно указать на существующий типизированный набор данных с помощью параметра `/r` средства Svcutil.exe. При попытке использовать Svcutil.exe без параметра `/r` применительно к службе, которая использует типизированный набор данных, автоматически выбирается альтернативный сериализатор (XmlSerializer). Если нужно использовать DataContractSerializer и создать наборы данных из схемы, можно воспользоваться следующей процедурой: создать типы типизированного набора данных (с помощью средства Xsd.exe с параметром `/d` применительно к службе), скомпилировать типы, а затем указать на них с помощью параметра `/r` средства Svcutil.exe.  
  
## <a name="see-also"></a>См. также  
 <xref:System.Runtime.Serialization.DataContractSerializer>  
 <xref:System.Xml.Serialization.IXmlSerializable>  
 [Использование контрактов данных](../../../../docs/framework/wcf/feature-details/using-data-contracts.md)  
 [Типы, поддерживаемые сериализатором контракта данных](../../../../docs/framework/wcf/feature-details/types-supported-by-the-data-contract-serializer.md)

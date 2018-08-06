---
title: 'Бессерверные приложения: архитектура, шаблоны и реализация в Azure'
description: Руководство по бессерверной архитектуре. Узнайте, когда, почему и как реализовать бессерверную архитектуру (в отличие от инфраструктуры как услуги [IaaS] или платформы как услуги [PaaS]) в корпоративных приложениях.
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 6/26/2018
ms.openlocfilehash: 89e5f387e218703a2f6311ef848b3d613a9279f7
ms.sourcegitcommit: 4c158beee818c408d45a9609bfc06f209a523e22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37404822"
---
![](./media/Cover.jpg)

# <a name="serverless-apps-architecture-patterns-and-azure-implementation"></a><span data-ttu-id="6b66f-104">Бессерверные приложения: архитектура, шаблоны и реализация в Azure</span><span class="sxs-lookup"><span data-stu-id="6b66f-104">Serverless apps: Architecture, patterns, and Azure implementation</span></span>

> <span data-ttu-id="6b66f-105">Можно загрузить по ссылке: <https://aka.ms/serverless-ebook></span><span class="sxs-lookup"><span data-stu-id="6b66f-105">DOWNLOAD available at: <https://aka.ms/serverless-ebook></span></span>

<span data-ttu-id="6b66f-106">ИЗДАТЕЛЬ</span><span class="sxs-lookup"><span data-stu-id="6b66f-106">PUBLISHED BY</span></span>

<span data-ttu-id="6b66f-107">Подразделение Microsoft Developer Division, команды разработки .NET и Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6b66f-107">Microsoft Developer Division, .NET, and Visual Studio product teams</span></span>

<span data-ttu-id="6b66f-108">Подразделение корпорации Майкрософт</span><span class="sxs-lookup"><span data-stu-id="6b66f-108">A division of Microsoft Corporation</span></span>

<span data-ttu-id="6b66f-109">One Microsoft Way</span><span class="sxs-lookup"><span data-stu-id="6b66f-109">One Microsoft Way</span></span>

<span data-ttu-id="6b66f-110">Redmond, Washington 98052-6399</span><span class="sxs-lookup"><span data-stu-id="6b66f-110">Redmond, Washington 98052-6399</span></span>

<span data-ttu-id="6b66f-111">© Корпорация Майкрософт (Microsoft Corporation), 2018.</span><span class="sxs-lookup"><span data-stu-id="6b66f-111">Copyright © 2018 by Microsoft Corporation</span></span>

<span data-ttu-id="6b66f-112">Все права защищены.</span><span class="sxs-lookup"><span data-stu-id="6b66f-112">All rights reserved.</span></span> <span data-ttu-id="6b66f-113">Запрещается полное или частичное воспроизведение или передача настоящей книги в любом виде или любыми средствами без письменного разрешения издателя.</span><span class="sxs-lookup"><span data-stu-id="6b66f-113">No part of the contents of this book may be reproduced or transmitted in any form or by any means without the written permission of the publisher.</span></span>

<span data-ttu-id="6b66f-114">Эта книга предоставляется на условиях "как есть" и выражает взгляды и мнения автора.</span><span class="sxs-lookup"><span data-stu-id="6b66f-114">This book is provided "as-is" and expresses the author's views and opinions.</span></span> <span data-ttu-id="6b66f-115">Взгляды, мнения и сведения, содержащиеся в этой книге, включая URL-адреса и другие ссылки на веб-сайты, могут изменяться без уведомления.</span><span class="sxs-lookup"><span data-stu-id="6b66f-115">The views, opinions and information expressed in this book, including URL and other Internet website references, may change without notice.</span></span>

<span data-ttu-id="6b66f-116">Некоторые приведенные в книге примеры служат только для иллюстрации и являются вымышленными.</span><span class="sxs-lookup"><span data-stu-id="6b66f-116">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="6b66f-117">Все совпадения с реальными наименованиями, людьми и любыми другими предметами являются непреднамеренными и случайными.</span><span class="sxs-lookup"><span data-stu-id="6b66f-117">No real association or connection is intended or should be inferred.</span></span>

<span data-ttu-id="6b66f-118">Microsoft и товарные знаки, перечисленные на странице "Товарные знаки" на сайте <https://www.microsoft.com>, являются товарными знаками группы компаний Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="6b66f-118">Microsoft and the trademarks listed at <https://www.microsoft.com> on the "Trademarks" webpage are trademarks of the Microsoft group of companies.</span></span>

<span data-ttu-id="6b66f-119">Mac и macOS являются товарными знаками Apple Inc.</span><span class="sxs-lookup"><span data-stu-id="6b66f-119">Mac and macOS are trademarks of Apple Inc.</span></span>

<span data-ttu-id="6b66f-120">Все другие наименования и логотипы являются собственностью своих законных владельцев.</span><span class="sxs-lookup"><span data-stu-id="6b66f-120">All other marks and logos are property of their respective owners.</span></span>

<span data-ttu-id="6b66f-121">Автор:</span><span class="sxs-lookup"><span data-stu-id="6b66f-121">Author:</span></span>

> <span data-ttu-id="6b66f-122">**[Джереми Ликнесс (Jeremy Likness)](https://twitter.com/jeremylikness)**, старший советник по облачной разработке, APEX, корпорация Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="6b66f-122">**[Jeremy Likness](https://twitter.com/jeremylikness)**, Sr. Cloud Developer Advocate, APEX, Microsoft Corp.</span></span>

<span data-ttu-id="6b66f-123">Участник:</span><span class="sxs-lookup"><span data-stu-id="6b66f-123">Contributor:</span></span>

> <span data-ttu-id="6b66f-124">**[Сесил Филлип (Cecil Phillip)](https://twitter.com/cecilphillip)**, советник по облачной разработке II, APEX, корпорация Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="6b66f-124">**[Cecil Phillip](https://twitter.com/cecilphillip)**, Cloud Developer Advocate II, APEX, Microsoft Corp.</span></span>

<span data-ttu-id="6b66f-125">Редакторы:</span><span class="sxs-lookup"><span data-stu-id="6b66f-125">Editors:</span></span>

> <span data-ttu-id="6b66f-126">**[Билл Вагнер (Bill Wagner)](https://twitter.com/billwagner)**, старший разработчик содержимого, APEX, корпорация Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="6b66f-126">**[Bill Wagner](https://twitter.com/billwagner)**, Senior Content Developer, APEX, Microsoft Corp.</span></span>

> <span data-ttu-id="6b66f-127">**[Майра Вензел (Maira Wenzel)](https://twitter.com/mairacw)**, старший разработчик содержимого, APEX, корпорация Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="6b66f-127">**[Maira Wenzel](https://twitter.com/mairacw)**, Senior Content Developer, APEX, Microsoft Corp.</span></span>

<span data-ttu-id="6b66f-128">Участники и рецензенты:</span><span class="sxs-lookup"><span data-stu-id="6b66f-128">Participants and reviewers:</span></span>

> <span data-ttu-id="6b66f-129">**[Стив Смит (Steve Smith)](https://twitter.com/ardalis)**, владелец, Ardalis Services.</span><span class="sxs-lookup"><span data-stu-id="6b66f-129">**[Steve Smith](https://twitter.com/ardalis)**, Owner, Ardalis Services.</span></span>

## <a name="introduction"></a><span data-ttu-id="6b66f-130">Вступление</span><span class="sxs-lookup"><span data-stu-id="6b66f-130">Introduction</span></span>

<span data-ttu-id="6b66f-131">Бессерверная архитектура — это следующий этап эволюции облачных платформ в направлении полностью облачного машинного кода.</span><span class="sxs-lookup"><span data-stu-id="6b66f-131">Serverless is the evolution of cloud platforms in the direction of pure cloud native code.</span></span> <span data-ttu-id="6b66f-132">С бессерверной архитектурой разработчики оказываются ближе к бизнес-логике и при этом могут не беспокоиться об инфраструктуре.</span><span class="sxs-lookup"><span data-stu-id="6b66f-132">Serverless brings developers closer to business logic while insulating them from infrastructure concerns.</span></span> <span data-ttu-id="6b66f-133">Этот шаблон подразумевает не отсутствие серверов, а меньшую зависимость от них.</span><span class="sxs-lookup"><span data-stu-id="6b66f-133">It's a pattern that doesn't imply "no server" but rather, "less server."</span></span> <span data-ttu-id="6b66f-134">Бессерверный код управляется событиями.</span><span class="sxs-lookup"><span data-stu-id="6b66f-134">Serverless code is event-driven.</span></span> <span data-ttu-id="6b66f-135">Код может запускаться чем угодно — от традиционных веб-запросов HTTP до таймера или результата передачи файла.</span><span class="sxs-lookup"><span data-stu-id="6b66f-135">Code may be triggered by anything from a traditional HTTP web request to a timer or the result of uploading a file.</span></span> <span data-ttu-id="6b66f-136">Инфраструктуру в бессерверной архитектуре можно моментально масштабировать в зависимости от потребностей, и вы платите дискретно, только за то, что действительно используете.</span><span class="sxs-lookup"><span data-stu-id="6b66f-136">The infrastructure behind serverless allows for instant scale to meet elastic demands and offers micro-billing to truly "pay for what you use."</span></span> <span data-ttu-id="6b66f-137">Бессерверная архитектура требует нового мышления и подхода к созданию приложений. Она не подходит для решения всех задач.</span><span class="sxs-lookup"><span data-stu-id="6b66f-137">Serverless requires a new way of thinking and approach to building applications and isn't the right solution for every problem.</span></span> <span data-ttu-id="6b66f-138">Как разработчик, вы должны решить:</span><span class="sxs-lookup"><span data-stu-id="6b66f-138">As a developer, you must decide:</span></span>

* <span data-ttu-id="6b66f-139">Каковы преимущества и недостатки бессерверной архитектуры?</span><span class="sxs-lookup"><span data-stu-id="6b66f-139">What are the pros and cons of serverless?</span></span>
* <span data-ttu-id="6b66f-140">Зачем вам нужна бессерверная архитектура для ваших приложений?</span><span class="sxs-lookup"><span data-stu-id="6b66f-140">Why should you consider serverless for your own applications?</span></span>
* <span data-ttu-id="6b66f-141">Как вы будете создавать, тестировать, развертывать и обслуживать бессерверный код?</span><span class="sxs-lookup"><span data-stu-id="6b66f-141">How can you build, test, deploy, and maintain your serverless code?</span></span>
* <span data-ttu-id="6b66f-142">Где в ваших приложениях имеет смысл перенести код в бессерверную архитектуру и как лучше всего осуществить это преобразование?</span><span class="sxs-lookup"><span data-stu-id="6b66f-142">Where does it make sense to migrate code to serverless in existing applications, and what is the best way to accomplish this transformation?</span></span>

## <a name="about-this-guide"></a><span data-ttu-id="6b66f-143">Об этом руководстве</span><span class="sxs-lookup"><span data-stu-id="6b66f-143">About this guide</span></span>

<span data-ttu-id="6b66f-144">Это руководство ориентировано на собственную разработку облачных приложений в бессерверной архитектуре.</span><span class="sxs-lookup"><span data-stu-id="6b66f-144">This guide focuses on cloud native development of applications that use serverless.</span></span> <span data-ttu-id="6b66f-145">В нем демонстрируются преимущества и потенциальные недостатки разработки бессерверных приложений и приводится обзор бессерверных архитектур.</span><span class="sxs-lookup"><span data-stu-id="6b66f-145">The book highlights the benefits and exposes the potential drawbacks of developing serverless apps and provides a survey of serverless architectures.</span></span> <span data-ttu-id="6b66f-146">Здесь вы найдете множество примеров использования бессерверной архитектуры и различные конструктивные шаблоны бессерверных приложений.</span><span class="sxs-lookup"><span data-stu-id="6b66f-146">Many examples of how serverless can be used are illustrated along with various serverless design patterns.</span></span>

<span data-ttu-id="6b66f-147">В этом руководстве описаны компоненты бессерверной платформы Azure и подробно рассказывается о реализации бессерверной архитектуры с помощью [Функций Azure](https://docs.microsoft.com/azure/azure-functions/functions-overview).</span><span class="sxs-lookup"><span data-stu-id="6b66f-147">This guide explains the components of the Azure serverless platform and focuses specifically on implementation of serverless using [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview).</span></span> <span data-ttu-id="6b66f-148">Вы узнаете о триггерах и привязках, а также о том, как реализовать бессерверные приложения, которые зависят от состояния, с помощью устойчивых функций.</span><span class="sxs-lookup"><span data-stu-id="6b66f-148">You'll learn about triggers and bindings as well as how to implement serverless apps that rely on state using durable functions.</span></span> <span data-ttu-id="6b66f-149">Наконец, сценарии и варианты использования для бизнеса предоставят необходимый контекст, который поможет вам принять решение о применении бессерверной архитектуры для своих проектов.</span><span class="sxs-lookup"><span data-stu-id="6b66f-149">Finally, business examples and case studies will help provide context and a frame of reference to determine whether serverless is the right approach for your projects.</span></span>

## <a name="evolution-of-cloud-platforms"></a><span data-ttu-id="6b66f-150">Эволюция облачных платформ</span><span class="sxs-lookup"><span data-stu-id="6b66f-150">Evolution of cloud platforms</span></span>

<span data-ttu-id="6b66f-151">Бессерверная архитектура — это кульминация нескольких этапов развития облачных платформ.</span><span class="sxs-lookup"><span data-stu-id="6b66f-151">Serverless is the culmination of several iterations of cloud platforms.</span></span> <span data-ttu-id="6b66f-152">Все началось с физического оборудования в центре обработки данных, а затем появились концепции инфраструктуры как услуги (IaaS) и платформы как услуги (PaaS).</span><span class="sxs-lookup"><span data-stu-id="6b66f-152">The evolution began with physical metal in the data center and progressed through Infrastructure as a Service (IaaS) and Platform as a Service (PaaS).</span></span>

![Эволюция от локальной до бессерверной архитектуры](./media/serverless-evolution-iaas-paas.png)

<span data-ttu-id="6b66f-154">До появления облачных платформ существовала четкая граница между разработкой и использованием.</span><span class="sxs-lookup"><span data-stu-id="6b66f-154">Before the cloud, a discernible boundary existed between development and operations.</span></span> <span data-ttu-id="6b66f-155">Для развертывания приложения приходилось отвечать на множество вопросов:</span><span class="sxs-lookup"><span data-stu-id="6b66f-155">Deploying an application meant answering myriad questions like:</span></span>

* <span data-ttu-id="6b66f-156">Какое оборудование установить?</span><span class="sxs-lookup"><span data-stu-id="6b66f-156">What hardware should be installed?</span></span>
* <span data-ttu-id="6b66f-157">Как ограничить физический доступ к компьютеру?</span><span class="sxs-lookup"><span data-stu-id="6b66f-157">How is physical access to the machine secured?</span></span>
* <span data-ttu-id="6b66f-158">Требуется ли источник бесперебойного питания (ИБП) для центра обработки данных?</span><span class="sxs-lookup"><span data-stu-id="6b66f-158">Does the data center require an Uninterruptible Power Supply (UPS)?</span></span>
* <span data-ttu-id="6b66f-159">Куда отправляются резервные копии хранилища?</span><span class="sxs-lookup"><span data-stu-id="6b66f-159">Where are storage backups sent?</span></span>
* <span data-ttu-id="6b66f-160">Там требуется резервное питание?</span><span class="sxs-lookup"><span data-stu-id="6b66f-160">Should there be redundant power?</span></span>

<span data-ttu-id="6b66f-161">Список можно продолжать, а затраты были огромными.</span><span class="sxs-lookup"><span data-stu-id="6b66f-161">The list goes on and the overhead was enormous.</span></span> <span data-ttu-id="6b66f-162">Во многих случаях ИТ-отделы мирились со значительными издержками.</span><span class="sxs-lookup"><span data-stu-id="6b66f-162">In many situations, IT departments were forced to deal with incredible waste.</span></span> <span data-ttu-id="6b66f-163">Издержки были связаны с выделением чрезмерного количества серверов и резервных компьютеров для аварийного восстановления и резервных серверов для горизонтального масштабирования. К счастью, с появлением технологии виртуализации (например, [Hyper-V](/virtualization/hyper-v-on-windows/about/)) и виртуальных машин возникла концепция инфраструктуры как услуги (IaaS).</span><span class="sxs-lookup"><span data-stu-id="6b66f-163">The waste was due to over-allocation of servers as backup machines for disaster recovery and standby servers to enable scale-out. Fortunately, the introduction of virtualization technology (like [Hyper-V](/virtualization/hyper-v-on-windows/about/)) with Virtual Machines (VMs) gave rise to Infrastructure as a Service (IaaS).</span></span> <span data-ttu-id="6b66f-164">Виртуализованная инфраструктура позволяла использовать основу из стандартного набора серверов и гибкую среду с возможностью выделения серверов по требованию.</span><span class="sxs-lookup"><span data-stu-id="6b66f-164">Virtualized infrastructure allowed operations to set up a standard set of servers as the backbone, leading to a flexible environment capable of provisioning unique servers "on demand."</span></span> <span data-ttu-id="6b66f-165">Что еще важнее, виртуализация подготовила почву для использования облака, где можно было предоставлять виртуальные машины как услугу.</span><span class="sxs-lookup"><span data-stu-id="6b66f-165">More important, virtualization set the stage for using the cloud to provide virtual machines "as a service."</span></span> <span data-ttu-id="6b66f-166">Компании могли больше не волноваться за резервное питание или физические компьютеры.</span><span class="sxs-lookup"><span data-stu-id="6b66f-166">Companies could easily get out of the business of worrying about redundant power or physical machines.</span></span> <span data-ttu-id="6b66f-167">Вместо этого они сосредоточились на виртуальной среде.</span><span class="sxs-lookup"><span data-stu-id="6b66f-167">Instead, they focused on the virtual environment.</span></span>

<span data-ttu-id="6b66f-168">IaaS по-прежнему требует высоких издержек, ведь компании все равно выполняют различные задачи.</span><span class="sxs-lookup"><span data-stu-id="6b66f-168">IaaS still requires heavy overhead because operations is still responsible for various tasks.</span></span> <span data-ttu-id="6b66f-169">К таким задачам относятся следующие.</span><span class="sxs-lookup"><span data-stu-id="6b66f-169">These tasks include:</span></span>

* <span data-ttu-id="6b66f-170">Исправление и резервное копирование серверов.</span><span class="sxs-lookup"><span data-stu-id="6b66f-170">Patching and backing up servers.</span></span>
* <span data-ttu-id="6b66f-171">Установка пакетов.</span><span class="sxs-lookup"><span data-stu-id="6b66f-171">Installing packages.</span></span>
* <span data-ttu-id="6b66f-172">Обновление операционной системы.</span><span class="sxs-lookup"><span data-stu-id="6b66f-172">Keeping the operating system up-to-date.</span></span>
* <span data-ttu-id="6b66f-173">Мониторинг приложения.</span><span class="sxs-lookup"><span data-stu-id="6b66f-173">Monitoring the application.</span></span>

<span data-ttu-id="6b66f-174">На следующем этапе эволюции удалось сократить издержки с помощью платформы как услуги (PaaS).</span><span class="sxs-lookup"><span data-stu-id="6b66f-174">The next evolution reduced the overhead by providing Platform as a Service (PaaS).</span></span> <span data-ttu-id="6b66f-175">С PaaS поставщик облачных служб сам занимается операционными системами, исправлениями системы безопасности и даже пакетами, необходимыми для поддержки определенной платформы.</span><span class="sxs-lookup"><span data-stu-id="6b66f-175">With PaaS, the cloud provider handles operating systems, security patches, and even the required packages to support a specific platform.</span></span> <span data-ttu-id="6b66f-176">Вместо того, чтобы создавать виртуальные машины, затем настраивать .NET Framework и устанавливать серверы служб IIS, разработчики просто выбирают целевую платформу, например веб-приложение или конечную точку API, и развертывают код напрямую.</span><span class="sxs-lookup"><span data-stu-id="6b66f-176">Instead of building a VM then configuring the .NET Framework and standing up Internet Information Services (IIS) servers, developers simply choose a "platform target" such as "web application" or "API endpoint" and deploy code directly.</span></span> <span data-ttu-id="6b66f-177">Вопросов об инфраструктуре стало меньше:</span><span class="sxs-lookup"><span data-stu-id="6b66f-177">The infrastructure questions are reduced to:</span></span>

* <span data-ttu-id="6b66f-178">Службы какого размера нам нужны?</span><span class="sxs-lookup"><span data-stu-id="6b66f-178">What size services are needed?</span></span>
* <span data-ttu-id="6b66f-179">Как выполнять горизонтальное масштабирование служб (добавлять больше серверов или узлов)?</span><span class="sxs-lookup"><span data-stu-id="6b66f-179">How do the services scale out (add more servers or nodes)?</span></span>
* <span data-ttu-id="6b66f-180">Как увеличить масштаб служб (увеличить емкость размещения серверов или узлов)?</span><span class="sxs-lookup"><span data-stu-id="6b66f-180">How do the services scale up (increase the capacity of hosting servers or nodes)?</span></span>

<span data-ttu-id="6b66f-181">Бессерверная архитектура еще меньше привязана к серверам и сосредоточена на коде, управляемом событиями.</span><span class="sxs-lookup"><span data-stu-id="6b66f-181">Serverless further abstracts servers by focusing on event-driven code.</span></span> <span data-ttu-id="6b66f-182">Разработчики занимаются не платформой, а отдельными микрослужбами с определенными функциями.</span><span class="sxs-lookup"><span data-stu-id="6b66f-182">Instead of a platform, developers can focus on a microservice that does one thing.</span></span> <span data-ttu-id="6b66f-183">У разработчиков бессерверного кода есть два главных вопроса:</span><span class="sxs-lookup"><span data-stu-id="6b66f-183">The two key questions for building the serverless code are:</span></span>

* <span data-ttu-id="6b66f-184">Что активирует код?</span><span class="sxs-lookup"><span data-stu-id="6b66f-184">What triggers the code?</span></span>
* <span data-ttu-id="6b66f-185">Что делает код?</span><span class="sxs-lookup"><span data-stu-id="6b66f-185">What does the code do?</span></span>

<span data-ttu-id="6b66f-186">Для бессерверного кода инфраструктура не так важна.</span><span class="sxs-lookup"><span data-stu-id="6b66f-186">With serverless, infrastructure is abstracted.</span></span> <span data-ttu-id="6b66f-187">В некоторых случаях разработчик вообще больше не думает о размещении.</span><span class="sxs-lookup"><span data-stu-id="6b66f-187">In some cases, the developer no longer worries about the host at all.</span></span> <span data-ttu-id="6b66f-188">Веб-запросами может управлять экземпляр IIS, Kestrel, Apache или другой веб-сервер, разработчик занимается только триггером HTTP.</span><span class="sxs-lookup"><span data-stu-id="6b66f-188">Whether or not an instance of IIS, Kestrel, Apache, or some other web server is running to manage web requests, the developer focuses on an HTTP trigger.</span></span> <span data-ttu-id="6b66f-189">Триггер предоставляет стандартные кроссплатформенные полезные данные для запроса.</span><span class="sxs-lookup"><span data-stu-id="6b66f-189">The trigger provides the standard, cross-platform payload for the request.</span></span> <span data-ttu-id="6b66f-190">Полезные данные упрощают не только процесс разработки, но и тестирование, а в некоторых случаях код можно легко переносить между платформами.</span><span class="sxs-lookup"><span data-stu-id="6b66f-190">The payload not only simplifies the development process, but facilitates testing and in some cases, makes the code easily portable across platforms.</span></span>

<span data-ttu-id="6b66f-191">Другая особенность бессерверной архитектуры — это дискретное выставление счетов.</span><span class="sxs-lookup"><span data-stu-id="6b66f-191">Another feature of serverless is micro-billing.</span></span> <span data-ttu-id="6b66f-192">Довольно часто веб-приложения размещают конечные точки веб-API.</span><span class="sxs-lookup"><span data-stu-id="6b66f-192">It's common for web applications to host Web API endpoints.</span></span> <span data-ttu-id="6b66f-193">В традиционных архитектурах без операционной системы, реализациях IaaS и даже PaaS ресурсы для размещения API-интерфейсов оплачиваются постоянно.</span><span class="sxs-lookup"><span data-stu-id="6b66f-193">In traditional bare metal, IaaS and even PaaS implementations, the resources to host the APIs are paid for continuously.</span></span> <span data-ttu-id="6b66f-194">Вы платите за размещение конечных точек, даже если не пользуетесь ими.</span><span class="sxs-lookup"><span data-stu-id="6b66f-194">That means you pay to host the endpoints even when they aren't being accessed.</span></span> <span data-ttu-id="6b66f-195">Обычно один API вызывается чаще других, поэтому вся система масштабируется с целью поддержки популярных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="6b66f-195">Often you'll find one API is called more than others, so the entire system is scaled based on supporting the popular endpoints.</span></span> <span data-ttu-id="6b66f-196">В бессерверной архитектуре конечные точки можно масштабировать независимо друг от друга и платить только за использование, а не за те API-интерфейсы, которые не вызываются.</span><span class="sxs-lookup"><span data-stu-id="6b66f-196">Serverless enables you to scale each endpoint independently and pay for usage, so no costs are incurred when the APIs aren't being called.</span></span> <span data-ttu-id="6b66f-197">Перенос обычно значительно сокращает текущие затраты на поддержку конечных точек.</span><span class="sxs-lookup"><span data-stu-id="6b66f-197">Migration may in many circumstances dramatically reduce the ongoing cost to support the endpoints.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="6b66f-198">Темы, которые выходят за рамки этого руководства</span><span class="sxs-lookup"><span data-stu-id="6b66f-198">What this guide doesn't cover</span></span>

<span data-ttu-id="6b66f-199">В этом руководстве описаны архитектурные подходы и конструктивные шаблоны и не рассматриваются детали реализации функций Azure, [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps) или других бессерверных платформ.</span><span class="sxs-lookup"><span data-stu-id="6b66f-199">This guide specifically emphasizes architecture approaches and design patterns and isn't a deep dive into the implementation details of Azure Functions, [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps), or other serverless platforms.</span></span> <span data-ttu-id="6b66f-200">В данном руководстве, например, не рассматриваются, расширенные рабочие процессы с Logic Apps или возможности функций Azure, такие как настройка общего доступа к ресурсам независимо от источника (CORS), применение личных доменов и передача SSL-сертификатов.</span><span class="sxs-lookup"><span data-stu-id="6b66f-200">This guide doesn't cover, for example, advanced workflows with Logic Apps or features of Azure Functions such as configuring Cross-Origin Resource Sharing (CORS), applying custom domains, or uploading SSL certificates.</span></span> <span data-ttu-id="6b66f-201">Эти сведения доступны в онлайн-документации по [функциям Azure](https://docs.microsoft.com/azure/azure-functions/functions-reference).</span><span class="sxs-lookup"><span data-stu-id="6b66f-201">These details are available through the online [Azure Functions documentation](https://docs.microsoft.com/azure/azure-functions/functions-reference).</span></span>

### <a name="additional-resources"></a><span data-ttu-id="6b66f-202">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6b66f-202">Additional resources</span></span>

* [<span data-ttu-id="6b66f-203">Центр архитектуры Azure</span><span class="sxs-lookup"><span data-stu-id="6b66f-203">Azure Architecture center</span></span>](https://docs.microsoft.com/azure/architecture/)
* [<span data-ttu-id="6b66f-204">Советы и рекомендации для облачных приложений</span><span class="sxs-lookup"><span data-stu-id="6b66f-204">Best practices for cloud applications</span></span>](https://docs.microsoft.com/azure/architecture/best-practices/api-design)

## <a name="who-should-use-the-guide"></a><span data-ttu-id="6b66f-205">Кому необходимо это руководство</span><span class="sxs-lookup"><span data-stu-id="6b66f-205">Who should use the guide</span></span>

<span data-ttu-id="6b66f-206">Данное руководство предназначено для разработчиков и архитекторов решений, занимающихся разработкой корпоративных приложений с помощью .NET, где можно использовать бессерверные компоненты локально или в облаке.</span><span class="sxs-lookup"><span data-stu-id="6b66f-206">This guide was written for developers and solution architects who want to build enterprise applications with .NET that may use serverless components either on premises or in the cloud.</span></span> <span data-ttu-id="6b66f-207">Оно полезно для разработчиков, архитекторов и технических руководителей, которые хотят:</span><span class="sxs-lookup"><span data-stu-id="6b66f-207">It's useful to developers, architects, and technical decision makers interested in:</span></span>

* <span data-ttu-id="6b66f-208">Понять плюсы и минусы бессерверной разработки</span><span class="sxs-lookup"><span data-stu-id="6b66f-208">Understanding the pros and cons of serverless development</span></span>
* <span data-ttu-id="6b66f-209">Научиться работать с бессерверной архитектурой</span><span class="sxs-lookup"><span data-stu-id="6b66f-209">Learning how to approach serverless architecture</span></span>
* <span data-ttu-id="6b66f-210">Изучить примеры реализации бессерверных приложений</span><span class="sxs-lookup"><span data-stu-id="6b66f-210">Example implementations of serverless apps</span></span>

## <a name="how-to-use-the-guide"></a><span data-ttu-id="6b66f-211">Как пользоваться руководством</span><span class="sxs-lookup"><span data-stu-id="6b66f-211">How to use the guide</span></span>

<span data-ttu-id="6b66f-212">В первой части руководства рассматривается, почему бессерверная архитектура имеет преимущества по сравнению с другими подходами.</span><span class="sxs-lookup"><span data-stu-id="6b66f-212">The first part of this guide examines why serverless is a viable option by comparing several different architecture approaches.</span></span> <span data-ttu-id="6b66f-213">Мы обсудим технологию и жизненный цикл разработки, поскольку выбор архитектуры влияет на все аспекты разработки программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="6b66f-213">It examines both the technology and development lifecycle, because all aspects of software development are impacted by architecture decisions.</span></span> <span data-ttu-id="6b66f-214">Затем в руководстве приводятся варианты использования и конструктивные шаблоны, а также примеры реализаций с помощью функций Azure.</span><span class="sxs-lookup"><span data-stu-id="6b66f-214">The guide then examines use cases and design patterns and includes reference implementations using Azure Functions.</span></span> <span data-ttu-id="6b66f-215">Каждый раздел содержит дополнительные ресурсы для детального изучения темы.</span><span class="sxs-lookup"><span data-stu-id="6b66f-215">Each section contains additional resources to learn more about a particular area.</span></span> <span data-ttu-id="6b66f-216">В конце мы приводим пошаговые руководства и практические исследования бессерверной реализации.</span><span class="sxs-lookup"><span data-stu-id="6b66f-216">The guide concludes with resources for walkthroughs and hands-on exploration of serverless implementation.</span></span>

## <a name="send-your-feedback"></a><span data-ttu-id="6b66f-217">Отправить отзыв</span><span class="sxs-lookup"><span data-stu-id="6b66f-217">Send your feedback</span></span>

<span data-ttu-id="6b66f-218">Руководство и примеры постоянно дополняются, поэтому мы ждем ваших отзывов.</span><span class="sxs-lookup"><span data-stu-id="6b66f-218">The guide and related samples are constantly evolving, so your feedback is welcomed!</span></span> <span data-ttu-id="6b66f-219">Если у вас есть комментарии о том, как можно улучшить это руководство, используйте раздел отзывов в нижней части любой страницы, созданный на основе [проблемы GitHub](https://github.com/dotnet/docs/issues).</span><span class="sxs-lookup"><span data-stu-id="6b66f-219">If you have comments about how this guide can be improved, use the feedback section at the bottom of any page built on [GitHub issues](https://github.com/dotnet/docs/issues).</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="6b66f-220">Вперед</span><span class="sxs-lookup"><span data-stu-id="6b66f-220">Next</span></span>](architecture-approaches.md)
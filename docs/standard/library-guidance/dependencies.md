---
title: Зависимости и библиотеки .NET
description: Рекомендации по управлению зависимостями NuGet в библиотеках .NET.
author: jamesnk
ms.author: mairaw
ms.date: 10/02/2018
ms.openlocfilehash: c5df30c606e77c9ef44387233b0072ab890f612f
ms.sourcegitcommit: e42d09e5966dd9fd02847d3e7eeb4ec0877069f8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2018
ms.locfileid: "49400554"
---
# <a name="dependencies"></a><span data-ttu-id="38764-103">Зависимости</span><span class="sxs-lookup"><span data-stu-id="38764-103">Dependencies</span></span>

<span data-ttu-id="38764-104">Основным средством для добавления зависимостей в библиотеку .NET являются ссылки на пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="38764-104">The primary way of adding dependencies to a .NET library is referencing NuGet packages.</span></span> <span data-ttu-id="38764-105">Ссылки на пакеты NuGet позволяют быстро подключить уже готовые функции, но для многих разработчиков .NET они часто становятся источником трудностей.</span><span class="sxs-lookup"><span data-stu-id="38764-105">NuGet package references allow you to quickly reuse and leverage already written functionality, but they're a common source of friction for .NET developers.</span></span> <span data-ttu-id="38764-106">Правильное управление зависимостями нужно для того, чтобы изменения в чужих библиотеках .NET не нарушали работу вашей библиотеки .NET, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="38764-106">Correctly managing dependencies is important to prevent changes in other .NET libraries from breaking your .NET library, and vice versa!</span></span>

## <a name="diamond-dependencies"></a><span data-ttu-id="38764-107">Ромбовидные зависимости</span><span class="sxs-lookup"><span data-stu-id="38764-107">Diamond dependencies</span></span>

<span data-ttu-id="38764-108">Широко распространены ситуации, когда в дереве зависимостей для проекта .NET существует несколько версий одного пакета.</span><span class="sxs-lookup"><span data-stu-id="38764-108">It's a common situation for a .NET project to have multiple versions of a package in its dependency tree.</span></span> <span data-ttu-id="38764-109">Например, если приложение зависит от двух пакетов NuGet, которые в свою очередь зависят от разных версий одного пакета.</span><span class="sxs-lookup"><span data-stu-id="38764-109">For example, an app depends on two NuGet packages, each of which depends on different versions of the same package.</span></span> <span data-ttu-id="38764-110">В схеме зависимостей это отображено в виде ромбовидной зависимости.</span><span class="sxs-lookup"><span data-stu-id="38764-110">A diamond dependency now exists in the app's dependency graph.</span></span>

<span data-ttu-id="38764-111">![Ромбовидная зависимость](./media/dependencies/diamond-dependency.png "Diamond dependency")</span><span class="sxs-lookup"><span data-stu-id="38764-111">![Diamond dependency](./media/dependencies/diamond-dependency.png "Diamond dependency")</span></span>

<span data-ttu-id="38764-112">Во время сборки NuGet анализирует все пакеты, от которых зависит проект, в том числе зависимости зависимостей.</span><span class="sxs-lookup"><span data-stu-id="38764-112">At build time, NuGet analyzes all the packages that a project depends on, including the dependencies of dependencies.</span></span> <span data-ttu-id="38764-113">Если в этом списке есть нескольких версий одного пакета, из них выбирается одна.</span><span class="sxs-lookup"><span data-stu-id="38764-113">When multiple versions of a package are detected, rules are evaluated to pick one.</span></span> <span data-ttu-id="38764-114">Унификация пакетов очень важна, так как в .NET очень сложно реализовать параллельную работу нескольких версий одной сборки в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="38764-114">Unifying packages is necessary because running side-by-side versions of an assembly in the same application is problematic in .NET.</span></span>

<span data-ttu-id="38764-115">Большинство ромбовидных зависимостей решаются очень легко, но в определенных обстоятельствах они могут создавать проблемы:</span><span class="sxs-lookup"><span data-stu-id="38764-115">Most diamond dependencies are easily resolved; however, they can create issues in certain circumstances:</span></span>

1. <span data-ttu-id="38764-116">**Конфликт ссылок на пакет NuGet** не позволяет разрешить конфликт версий при восстановлении пакетов.</span><span class="sxs-lookup"><span data-stu-id="38764-116">**Conflicting NuGet package references** prevent a version from being resolved during package restore.</span></span>
2. <span data-ttu-id="38764-117">**Критические изменения между версиями** приводят к ошибкам и исключениям во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="38764-117">**Breaking changes between the versions** cause bugs and exceptions at runtime.</span></span>
3. <span data-ttu-id="38764-118">**Строгое именование пакета сборки**, если изменилась версия сборки для приложения, выполняющегося на .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="38764-118">**The package assembly is strong named**, the assembly version changed, and the app is running on the .NET Framework.</span></span> <span data-ttu-id="38764-119">В такой ситуации требуется переадресация привязок.</span><span class="sxs-lookup"><span data-stu-id="38764-119">Assembly binding redirects are required.</span></span>

<span data-ttu-id="38764-120">Невозможно предсказать, какие пакеты будут работать параллельно с вашим.</span><span class="sxs-lookup"><span data-stu-id="38764-120">It's not possible to know what packages will be used alongside your own.</span></span> <span data-ttu-id="38764-121">Чтобы снизить вероятность проблем с библиотекой при возникновении ромбовидной зависимости, сократите до минимума число пакетов, от которых она зависит.</span><span class="sxs-lookup"><span data-stu-id="38764-121">A good way to reduce the likelihood of a diamond dependency breaking your library is to minimize the number of packages you depend on.</span></span>

<span data-ttu-id="38764-122">**✔️ РЕКОМЕНДУЕТСЯ.** Проверьте наличие ненужных зависимостей в библиотеке .NET.</span><span class="sxs-lookup"><span data-stu-id="38764-122">**✔️ DO** review your .NET library for unnecessary dependencies.</span></span>

## <a name="nuget-dependency-version-ranges"></a><span data-ttu-id="38764-123">Диапазон версий для зависимостей NuGet</span><span class="sxs-lookup"><span data-stu-id="38764-123">NuGet dependency version ranges</span></span>

<span data-ttu-id="38764-124">Ссылка на пакет определяет диапазон допустимых пакетов.</span><span class="sxs-lookup"><span data-stu-id="38764-124">A package reference specifies the range of valid packages it allows.</span></span> <span data-ttu-id="38764-125">Как правило, ссылка в файле проекта определяет минимально необходимую версию пакета, а верхний предел отсутствует.</span><span class="sxs-lookup"><span data-stu-id="38764-125">Typically, the package reference version in the project file is the minimum version and there's no maximum.</span></span>

```xml
<!-- Accepts any version 1.0 and above. -->
<PackageReference Include="ExamplePackage" Version="1.0" />
```

<span data-ttu-id="38764-126">NuGet использует достаточно [сложные](/nuget/consume-packages/dependency-resolution) правила при разрешении зависимостей, всегда пытаясь найти минимально приемлемую версию.</span><span class="sxs-lookup"><span data-stu-id="38764-126">The rules that NuGet uses when resolving dependencies are [complex](/nuget/consume-packages/dependency-resolution), but NuGet always looks for the lowest applicable version.</span></span> <span data-ttu-id="38764-127">NuGet предпочитает минимально приемлемую версию, а не максимально доступную, чтобы снизить риск проблем с совместимостью.</span><span class="sxs-lookup"><span data-stu-id="38764-127">NuGet prefers the lowest applicable version over using the highest available because the lowest will have the least compatibility issues.</span></span>

<span data-ttu-id="38764-128">Это правило минимально приемлемых версий позволяет не указывать верхний предел для диапазона версий пакета, даже если вы хотите избежать получения последней версии.</span><span class="sxs-lookup"><span data-stu-id="38764-128">Because of NuGet's lowest applicable version rule, it isn't necessary to place an upper version or exact range on package references to avoid getting the latest version.</span></span> <span data-ttu-id="38764-129">NuGet и без этого будет искать наименьшую, то есть максимально совместимую, версию пакета.</span><span class="sxs-lookup"><span data-stu-id="38764-129">NuGet already tries to find the lowest, most compatible version for you.</span></span>

```xml
<!-- Accepts 1.0 up to 1.x, but not 2.0 and higher. -->
<PackageReference Include="ExamplePackage" Version="[1.0,2.0)" />

<!-- Accepts exactly 1.0. -->
<PackageReference Include="ExamplePackage" Version="[1.0]" />
```

<span data-ttu-id="38764-130">Наличие верхнего предела для версий пакета приведет к сбою NuGet в случае конфликта зависимостей.</span><span class="sxs-lookup"><span data-stu-id="38764-130">Upper version limits will cause NuGet to fail if there's a conflict.</span></span> <span data-ttu-id="38764-131">Предположим, одна из библиотек требует строго версию 1.0, а другая — версию не ниже 2.0.</span><span class="sxs-lookup"><span data-stu-id="38764-131">For example, one library accepts exactly 1.0 while another library requires 2.0 or above.</span></span> <span data-ttu-id="38764-132">Использование версии 2.0 может привести к ошибкам, если в ней внесены критические изменения. Но строгое верхнее ограничение гарантирует ошибку.</span><span class="sxs-lookup"><span data-stu-id="38764-132">While breaking changes may have been introduced in version 2.0, a strict or upper limit version dependency guarantees an error.</span></span>

<span data-ttu-id="38764-133">![Конфликт ромбовидных зависимостей](./media/dependencies/diamond-dependency-conflict.png "Diamond dependency conflict")</span><span class="sxs-lookup"><span data-stu-id="38764-133">![Diamond dependency conflict](./media/dependencies/diamond-dependency-conflict.png "Diamond dependency conflict")</span></span>

<span data-ttu-id="38764-134">**❌ НЕЛЬЗЯ.** Не используйте ссылки на пакет NuGet без минимально допустимой версии.</span><span class="sxs-lookup"><span data-stu-id="38764-134">**❌ DO NOT** have NuGet package references with no minimum version.</span></span>

<span data-ttu-id="38764-135">**❌ НЕЖЕЛАТЕЛЬНО.** Не используйте ссылки на пакеты NuGet, требующие указания точной версии.</span><span class="sxs-lookup"><span data-stu-id="38764-135">**❌ AVOID** NuGet package references that demand an exact version.</span></span>

<span data-ttu-id="38764-136">**❌ НЕЖЕЛАТЕЛЬНО.** Не используйте ссылки на пакет NuGet с указанием верхнего предела.</span><span class="sxs-lookup"><span data-stu-id="38764-136">**❌ AVOID** NuGet package references with a version upper limit.</span></span>

## <a name="nuget-shared-source-packages"></a><span data-ttu-id="38764-137">Совместно используемые пакеты кода NuGet</span><span class="sxs-lookup"><span data-stu-id="38764-137">NuGet shared source packages</span></span>

<span data-ttu-id="38764-138">Чтобы снизить внешние зависимости пакета NuGet, можно использовать ссылки на совместно используемые пакеты кода.</span><span class="sxs-lookup"><span data-stu-id="38764-138">One way to reduce external NuGet package dependencies is to reference shared source packages.</span></span> <span data-ttu-id="38764-139">Совместно используемые пакеты кода содержат [файлы исходного кода](/nuget/reference/nuspec#including-content-files), которые включаются в проект по ссылке.</span><span class="sxs-lookup"><span data-stu-id="38764-139">A shared source package contains [source code files](/nuget/reference/nuspec#including-content-files) that are included in a project when referenced.</span></span> <span data-ttu-id="38764-140">Так как эти файлы исходного кода напрямую включаются в проект и компилируются вместе с остальной частью проекта, не возникает никаких внешних зависимостей и возможностей для конфликта.</span><span class="sxs-lookup"><span data-stu-id="38764-140">Because you're just including source code files that are compiled with the rest of your project, there's no external dependency and chance of conflict.</span></span>

<span data-ttu-id="38764-141">Совместно используемые пакеты кода — это отличный способ включать небольшие фрагменты и функции.</span><span class="sxs-lookup"><span data-stu-id="38764-141">Shared source packages are great for including small pieces of functionality.</span></span> <span data-ttu-id="38764-142">Например, совместно используемый пакет кода может содержать вспомогательные методы для выполнения вызовов HTTP.</span><span class="sxs-lookup"><span data-stu-id="38764-142">For example, a shared source package of helper methods for making HTTP calls.</span></span>

<span data-ttu-id="38764-143">![Совместно используемый пакет кода](./media/dependencies/shared-source-package.png "Shared source package")</span><span class="sxs-lookup"><span data-stu-id="38764-143">![Shared source package](./media/dependencies/shared-source-package.png "Shared source package")</span></span>

```xml
<PackageReference Include="Microsoft.Extensions.Buffers.Testing.Sources" PrivateAssets="All" Version="1.0" />
```

<span data-ttu-id="38764-144">![Совместно используемый проект кода](./media/dependencies/shared-source-project.png "Shared source project")</span><span class="sxs-lookup"><span data-stu-id="38764-144">![Shared source project](./media/dependencies/shared-source-project.png "Shared source project")</span></span>

<span data-ttu-id="38764-145">Совместно используемые пакеты кода имеют ряд ограничений.</span><span class="sxs-lookup"><span data-stu-id="38764-145">Shared source packages have some limitations.</span></span> <span data-ttu-id="38764-146">Для них можно использовать только ссылки `PackageReference`, поэтому более старые проекты `packages.config` исключаются.</span><span class="sxs-lookup"><span data-stu-id="38764-146">They can only be referenced by `PackageReference`, so older `packages.config` projects are excluded.</span></span> <span data-ttu-id="38764-147">Также совместно используемые пакеты кода можно применять только в проектах с тем же типом языка.</span><span class="sxs-lookup"><span data-stu-id="38764-147">Also shared source packages are only usable by projects with the same language type.</span></span> <span data-ttu-id="38764-148">Из-за этих ограничений совместно используемые пакеты кода лучше всего подходят для совместного использования функций в рамках проекта с открытым исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="38764-148">Because of these limitations shared source packages are best used to share functionality within an open-source project.</span></span>

<span data-ttu-id="38764-149">**✔️ ДОПУСТИМО.** Вы можете использовать ссылки на совместно используемых пакетов кода для небольших встроенных функций.</span><span class="sxs-lookup"><span data-stu-id="38764-149">**✔️ CONSIDER** referencing shared source packages for small, internal pieces of functionality.</span></span>

<span data-ttu-id="38764-150">**✔️ ДОПУСТИМО.** Вы можете создать совместно используемый пакет кода, если он предоставляет небольшие встроенные функции.</span><span class="sxs-lookup"><span data-stu-id="38764-150">**✔️ CONSIDER** making your package a shared source package if it provides small, internal pieces of functionality.</span></span>

<span data-ttu-id="38764-151">**✔️ РЕКОМЕНДУЕТСЯ.** Указывайте ссылку `PrivateAssets="All"` для совместно используемых пакетов кода.</span><span class="sxs-lookup"><span data-stu-id="38764-151">**✔️ DO** reference shared source packages with `PrivateAssets="All"`.</span></span>

> <span data-ttu-id="38764-152">Этот вариант сообщает NuGet, что пакет будет использоваться только во время разработки и не должен предоставляться как открытая зависимость.</span><span class="sxs-lookup"><span data-stu-id="38764-152">This setting tells NuGet the package is only to be used at development time and shouldn't be exposed as a public dependency.</span></span>

<span data-ttu-id="38764-153">**❌ НЕЛЬЗЯ.** Не используйте совместно используемые пакеты кода в общедоступном API.</span><span class="sxs-lookup"><span data-stu-id="38764-153">**❌ DO NOT** have shared source package types in your public API.</span></span>

> <span data-ttu-id="38764-154">Совместно используемые типы компилируются прямо в сборку, которая содержит ссылку на них, и их невозможно передать в другую сборку.</span><span class="sxs-lookup"><span data-stu-id="38764-154">Shared source types are compiled into the referencing assembly and can't be exchanged across assembly boundaries.</span></span> <span data-ttu-id="38764-155">Например, совместно используемый тип `IRepository` в одном проекте не будет совпадать с тем же совместно используемым типом `IRepository` в другом проекте.</span><span class="sxs-lookup"><span data-stu-id="38764-155">For example, a shared-source `IRepository` type in one project is a separate type from the same shared-source `IRepository` in another project.</span></span> <span data-ttu-id="38764-156">Типы в совместно используемых пакетах кода должны иметь предел видимости `internal`.</span><span class="sxs-lookup"><span data-stu-id="38764-156">Types in shared source packages should have an `internal` visibility.</span></span>

<span data-ttu-id="38764-157">**❌ НЕЛЬЗЯ.** Не публикуйте совместно используемые пакеты кода на сайте nuget.org.</span><span class="sxs-lookup"><span data-stu-id="38764-157">**❌ DO NOT** publish shared source packages to nuget.org.</span></span>

> <span data-ttu-id="38764-158">Совместно используемые пакеты кода содержат исходный код и могут использоваться только в проектах с тем же типом языка.</span><span class="sxs-lookup"><span data-stu-id="38764-158">Shared source packages contain source code and can only be used by projects with the same language type.</span></span> <span data-ttu-id="38764-159">Например, совместно используемые пакеты кода на C# невозможно использовать в приложении F#.</span><span class="sxs-lookup"><span data-stu-id="38764-159">For example, a C# shared source package cannot be used by an F# application.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="38764-160">[Назад](./nuget.md)
[Вперед](./sourcelink.md)</span><span class="sxs-lookup"><span data-stu-id="38764-160">[Previous](./nuget.md)
[Next](./sourcelink.md)</span></span>
---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/15/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 634143f56bc9134f240ff42b7b5989605c8bffde
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45979462"
---
## <a name="set-up-the-development-environment"></a>De ontwikkelomgeving instellen

In deze sectie helpt bij het instellen van de ontwikkelomgeving. Dit omvat het maken van een ASP.NET MVC-app, de verbinding van een verbonden services toe te voegen, een controller toe te voegen en de vereiste naamruimte richtlijnen op te geven.

### <a name="create-an-aspnet-mvc-app-project"></a>Een ASP.NET MVC-app-project maken

1. Open Visual Studio.

1. Selecteer in het hoofdmenu van **bestand** > **nieuw** > **Project**.

1. In de **nieuw Project** in het dialoogvenster, selecteer **Web** > **ASP.NET-webtoepassing (.NET Framework)**. In de **naam** veld, geeft u **StorageAspNet**. Selecteer **OK**.

    ![Schermopname van nieuw Project-dialoogvenster](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. In de **nieuwe ASP.NET-webtoepassing** in het dialoogvenster, selecteer **MVC**, en selecteer vervolgens **OK**.

    ![Schermopname van nieuw ASP.NET-webtoepassing-dialoogvenster](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a>Verbonden services gebruiken voor verbinding met een Azure storage-account

1. In **Solution Explorer**, met de rechtermuisknop op het project.

2. Selecteer in het contextmenu **toevoegen** > **Connected Service**.

1. In de **Connected Services** in het dialoogvenster, selecteer **opslag in de Cloud met Azure Storage**.

    ![In het dialoogvenster Schermafbeelding van de verbonden Services](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. In de **Azure Storage** in het dialoogvenster, selecteer het Azure storage-account moet worden gebruikt voor deze zelfstudie. Voor het maken van een nieuw Azure storage-account selecteert **maken van een nieuw Opslagaccount**, en vul het formulier. Selecteer na het selecteren van een bestaand opslagaccount of het maken van een nieuwe, **toevoegen**. Visual Studio installeert het NuGet-pakket voor Azure Storage en een verbindingsreeks voor opslag op **Web.config**.

> [!TIP]
> Voor meer informatie over het maken van een storage-account met de [Azure-portal](https://portal.azure.com), Zie [een opslagaccount maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account).
>
> U kunt ook een storage-account maken met behulp van [Azure PowerShell](../articles/storage/common/storage-powershell-guide-full.md), [Azure CLI](../articles/storage/common/storage-azure-cli.md), of [Azure Cloud Shell](../articles/cloud-shell/overview.md).


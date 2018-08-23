---
title: Aan de slag met Key Vault Connected Service in Visual Studio (ASP.NET-projecten) | Microsoft Docs
description: Gebruik deze zelfstudie voor meer informatie over hoe u Key Vault-ondersteuning toevoegt aan een ASP.NET- of ASP.NET Core-webtoepassing.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: 3ca62d47d8e7682c80985bf5409b8540382fbf45
ms.sourcegitcommit: 30c7f9994cf6fcdfb580616ea8d6d251364c0cd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2018
ms.locfileid: "42054512"
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-projects"></a>Aan de slag met Key Vault Connected Service in Visual Studio (ASP.NET-projecten)

> [!div class="op_single_selector"]
> - [Aan de slag](vs-key-vault-aspnet-get-started.md)
> - [Wat is er gebeurd](vs-key-vault-aspnet-what-happened.md)

In dit artikel vindt u aanvullende richtlijnen nadat u Key Vault hebt toegevoegd aan een ASP.NET MVC-project via de **Connected Services toevoegen** opdracht in Visual Studio. Als u hebt de service nog niet is toegevoegd aan uw project, kunt u dat doen op elk gewenst moment door de instructies in [Key Vault toevoegen aan uw webtoepassing met behulp van Visual Studio Connected Services](vs-key-vault-add-connected-service.md).

Zie [wat is er gebeurd met mijn ASP.NET-project?](vs-key-vault-aspnet-core-what-happened.md) voor de wijzigingen aan uw project bij het toevoegen van de gekoppelde service.

## <a name="after-you-connect"></a>Nadat u verbinding hebt gemaakt

1. Een geheim toevoegen in uw Key Vault in Azure. Als u naar de juiste plaats in de portal, klik op de koppeling voor **geheimen die zijn opgeslagen in deze Key Vault beheren**. Als u de pagina of het project hebt gesloten, u kunt navigeren in de [Azure-portal](https://portal.azure.com) door te kiezen **alle Services**onder **Security**, kiest u **Key Vault**, en kies vervolgens de Sleutelkluis die u zojuist hebt gemaakt.

   ![Navigeren naar de portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. In de sectie Key Vault voor de key vault u hebt gemaakt, kiest u **geheimen**, klikt u vervolgens **genereren/importeren**.

   ![Een geheim genereren/importeren](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Voer een geheim, zoals **MySecret**, en wijs hieraan een string-waarde als een test, en kies vervolgens de **maken** knop.

   ![Een geheim maken](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. (optioneel) Geef een andere geheim, maar deze keer wordt in een categorie door de naam van het **geheimen--MySecret**. Deze syntaxis Hiermee geeft u een categorie **geheimen** die een geheim bevat **MySecret**.

1. Web.config als volgt wijzigen. De sleutels zijn tijdelijke aanduidingen die wordt vervangen door de AzureKeyVault ConfigurationBuilder met de waarden van geheimen in Key Vault.

   ```xml
     <appSettings configBuilders="AzureKeyVault">
       <add key="webpages:Version" value="3.0.0.0" />
       <add key="webpages:Enabled" value="false" />
       <add key="ClientValidationEnabled" value="true" />
       <add key="UnobtrusiveJavaScriptEnabled" value="true" />
       <add key="MySecret" value="dummy1"/>
       <add key="Secrets--MySecret" value="dummy2"/>
     </appSettings>
   ```

1. Voeg de volgende regels voor het ophalen van de geheime sleutel en sla deze op in de ViewBag in de HomeController, in de methode van de domeincontroller over.
 
   ```csharp
            var secret = ConfigurationManager.AppSettings["MySecret"];
            var secret2 = ConfigurationManager.AppSettings["Secrets--MySecret"];
            ViewBag.Secret = $"Secret: {secret}";
            ViewBag.Secret2 = $"Secret2: {secret2}";
   ```

1. Voeg de volgende om de waarde van het geheim (alleen voor testdoeleinden) weer te geven in de weergave About.cshtml.

   ```csharp
      <h3>@ViewBag.Secret</h3>
      <h3>@ViewBag.Secret2</h3>
   ```

Gefeliciteerd, u hebt nu uw web-app voor Key Vault gebruiken voor toegang tot geheimen veilig opgeslagen ingeschakeld.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep. Hiermee verwijdert u de Key Vault en de gerelateerde resources. De resourcegroep verwijderen via de portal:

1. Voer de naam van uw resourcegroep in het vak Zoeken bovenaan de portal in. Wanneer u de in deze snelstart gebruikte resourcegroep in de zoekresultaten ziet, selecteert u deze.
2. Selecteer **Resourcegroep verwijderen**.
3. Typ in het vak **TYP DE NAAM VAN DE RESOURCEGROEP** de naam van de resourcegroep en selecteer **Verwijderen**.

# <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen met Key Vault in de [Ontwikkelaarshandleiding voor Key Vault](key-vault-developers-guide.md)
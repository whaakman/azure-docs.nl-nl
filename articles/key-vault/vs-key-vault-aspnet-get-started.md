---
title: Aan de slag met Key Vault verbonden Service in Visual Studio (ASP.NET-projecten) | Microsoft Docs
description: Met deze zelfstudie kunt u informatie over het toevoegen van ondersteuning voor Sleutelkluis naar een webtoepassing ASP.NET of ASP.NET Core.
services: key-vault
author: ghogen
manager: douge
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.workload: azure
ms.topic: conceptual
ms.date: 04/15/2018
ms.author: ghogen
ms.openlocfilehash: cd305801f10c899682aa6d751e48f30b6e8303fa
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="get-started-with-key-vault-connected-service-in-visual-studio-aspnet-projects"></a>Aan de slag met Key Vault verbonden Service in Visual Studio (ASP.NET-projecten)

> [!div class="op_single_selector"]
> - [Aan de slag](vs-key-vault-aspnet-get-started.md)
> - [Wat is er gebeurd](vs-key-vault-aspnet-what-happened.md)

Dit artikel vindt u aanvullende richtlijnen nadat u Sleutelkluis hebt toegevoegd aan een ASP.NET MVC-project via de **verbonden Services toevoegen** opdracht in Visual Studio. Als u hebt de service niet is toegevoegd aan uw project, u kunt dit doen op elk gewenst moment door de instructies in [Sleutelkluis toevoegen aan uw webtoepassing met behulp van Visual Studio verbonden Services](vs-key-vault-add-connected-service.md).

Zie [wat is er gebeurd met mijn ASP.NET-project?](vs-key-vault-aspnet-core-what-happened.md) de wijzigingen aan uw project in bij het toevoegen van de gekoppelde service.

## <a name="after-you-connect"></a>Nadat u verbinding hebt gemaakt

1. Voeg een geheim in uw Sleutelkluis in Azure. Als u op de juiste plaats in de portal, klikt u op de koppeling voor **beheren geheimen die zijn opgeslagen in deze Sleutelkluis**. Als u de pagina of het project hebt gesloten, u kunt naar navigeren in de [Azure-portal](https://portal.azure.com) door te kiezen **alle Services**onder **beveiliging**, kies **Sleutelkluis**, kiest u de Sleutelkluis die u zojuist hebt gemaakt.

   ![Navigeren naar de portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. In de sectie Sleutelkluis voor de sleutel kluis hebt gemaakt, kiest u **geheimen**, klikt u vervolgens **Generate/importeren**.

   ![Een geheim genereren/importeren](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Voer een geheim, zoals **MySecret**, en wijs hieraan de waarde van een tekenreeks als een test en kies vervolgens de **maken** knop.

   ![Een geheim maken](media/vs-key-vault-add-connected-service/create-a-secret.jpg)
 
1. (optioneel) Geef een andere geheim, maar deze keer plaatsen in een categorie door de naam van het **geheimen--MySecret**. Deze syntaxis Hiermee geeft u een categorie **geheimen** die een geheim bevat **MySecret**.

1. Web.config als volgt wijzigen. De sleutels zijn tijdelijke aanduidingen worden vervangen door de AzureKeyVault ConfigurationBuilder met de waarden van geheimen in de Sleutelkluis.

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

1. Voeg de volgende regels van het geheim ophalen en opslaan in de ViewBag in de HomeController, in de methode van de domeincontroller over.
 
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

Gefeliciteerd, u hebt nu uw web-app op Sleutelkluis voor toegang tot veilig opgeslagen geheimen ingeschakeld.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep. Hiermee verwijdert u de Sleutelkluis en de bijbehorende resources. De resourcegroep verwijderen via de portal:

1. Voer de naam van uw resourcegroep in het vak Zoeken bovenaan de portal in. Wanneer u de in deze snelstart gebruikte resourcegroep in de zoekresultaten ziet, selecteert u deze.
2. Selecteer **Resourcegroep verwijderen**.
3. Typ in het vak **TYP DE NAAM VAN DE RESOURCEGROEP** de naam van de resourcegroep en selecteer **Verwijderen**.

# <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen met Sleutelkluis in de [Sleutelkluis Developer Guide](key-vault-developers-guide.md)
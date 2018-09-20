---
title: Ondersteuning voor Key Vault toevoegen aan uw ASP.NET-project met behulp van Visual Studio | Microsoft Docs
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
ms.openlocfilehash: 9cf49ae97da3bf67300bdc222c86bb712aeaed37
ms.sourcegitcommit: 06724c499837ba342c81f4d349ec0ce4f2dfd6d6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46465789"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Key Vault toevoegen aan uw web-App met behulp van Visual Studio verbonden Services

In deze zelfstudie leert u hoe eenvoudig u alles wat die u nodig hebt om te starten met Azure Key Vault voor het beheren van uw geheimen voor webprojecten in Visual Studio, of u van ASP.NET Core of elk type ASP.NET-project gebruikmaakt toevoegt. U kunt met behulp van de functie voor Services die zijn verbonden in Visual Studio 2017, Visual Studio automatisch toevoegen van de NuGet-pakketten en configuratie-instellingen die u wilt verbinding maken met Key Vault in Azure hebben. 

Zie voor meer informatie over de wijzigingen Connected Services voor het inschakelen van Key Vault maakt in uw project, [Key Vault Connected Service - wat is er gebeurd met mijn ASP.NET 4.7.1 project](vs-key-vault-aspnet-what-happened.md) of [Key Vault Connected Service - wat is er gebeurd met mijn ASP.NET Core-project](vs-key-vault-aspnet-core-what-happened.md).

## <a name="prerequisites"></a>Vereisten

- **Een Azure-abonnement**. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 versie 15,7** met de **webontwikkeling** werkbelasting geïnstalleerd. [Nu downloaden](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Voor ASP.NET (niet-Core) moet u de .NET Framework 4.7.1 ontwikkelhulpprogramma's, die niet standaard worden geïnstalleerd. Als u wilt installeren, start het installatieprogramma van Visual Studio, kiest u **wijzigen**, en kies vervolgens **afzonderlijke onderdelen**, vouw vervolgens aan de rechterkant **ASP.NET en webontwikkeling**, en kies **.NET Framework 4.7.1 ontwikkelprogramma's**.
- Een ASP.NET 4.7.1 of ASP.NET Core 2.0 webproject openen.

## <a name="add-key-vault-support-to-your-project"></a>Key Vault-ondersteuning aan uw project toevoegen

1. In **Solution Explorer**, kiest u **toevoegen** > **Connected Service**.
   De Connected Service-pagina wordt weergegeven met services die u aan uw project toevoegen kunt.
1. Kies in het menu van de beschikbare services, **beveiligde geheimen met Azure Key Vault**.

   ![Kies "Beveiligde geheimen met Azure Key Vault"](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

   Als u bent aangemeld bij Visual Studio en Azure-abonnement dat is gekoppeld aan uw account hebt, wordt een pagina weergegeven met een vervolgkeuzelijst met uw abonnementen. Zorg ervoor dat u bent aangemeld bij Visual Studio, en dat het account dat u bent aangemeld met hetzelfde account dat u voor uw Azure-abonnement gebruiken.

1. Selecteer het abonnement dat u wilt gebruiken, en kies vervolgens een nieuwe of bestaande Key Vault of kies de koppeling bewerken om te wijzigen van de automatisch gegenereerde naam.

   ![Selecteer uw abonnement](media/vs-key-vault-add-connected-service/KeyVaultConnectedService3.PNG)

1. Typ de naam die u wilt gebruiken voor de Key Vault.

   ![Wijzig de naam van de Key Vault en kies een resourcegroep](media/vs-key-vault-add-connected-service/KeyVaultConnectedService-Edit.PNG)

1. Selecteer een bestaande resourcegroep of een nieuwe maken met een automatisch gegenereerde unieke naam.  Als u een nieuwe groep maken met een andere naam wilt, kunt u de [Azure Portal](https://portal.azure.com), en de pagina te sluiten en opnieuw opstarten om opnieuw te laden van de lijst met resourcegroepen.
1. Kies de regio waarin u wilt maken van de Key Vault. Als uw webtoepassing wordt gehost in Azure, kiest u de regio die als host fungeert voor de web-App voor optimale prestaties.
1. Kies een prijsmodel. Zie voor meer informatie, [prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/).
1. Klik op OK om te accepteren van de configuratie-opties.
1. Kies **toevoegen** te maken van de Key Vault. Het proces maken kan mislukken als u ervoor kiest een naam die is gebruikt.  Als dit gebeurt, gebruikt u de **bewerken** koppeling naar de naam van de Key Vault en probeer het opnieuw.

   ![Verbonden service toevoegen aan project](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Nu u een geheim toevoegen in uw Key Vault in Azure. Als u naar de juiste plaats in de portal, klik op de koppeling voor beheren geheimen die zijn opgeslagen in deze Key Vault. Als u de pagina of het project hebt gesloten, u kunt navigeren in de [Azure-portal](https://portal.azure.com) door te kiezen **alle Services**onder **Security**, kiest u **Key Vault**, en kies vervolgens de Sleutelkluis die u zojuist hebt gemaakt.

   ![Navigeren naar de portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. In de sectie Key Vault voor de key vault u hebt gemaakt, kiest u **geheimen**, klikt u vervolgens **genereren/importeren**.

   ![Een geheim genereren/importeren](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Voer een geheim, zoals 'MySecret', en wijs hieraan een string-waarde als een test en kies vervolgens de **maken** knop.

   ![Een geheim maken](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (optioneel) Voer een andere geheim, maar deze keer plaatsen deze in een categorie door de naam van het 'Geheimen--MySecret'. Deze syntaxis Hiermee geeft u een categorie 'Geheimen' met een geheim "MySecret."
 
U kunt nu toegang tot uw geheimen in de code. De volgende stappen zijn verschillend, afhankelijk van of u van ASP.NET 4.7.1 of ASP.NET Core gebruikmaakt.

## <a name="access-your-secrets-in-code-aspnet-core-projects"></a>Toegang tot uw geheimen in de code (ASP.NET Core-projecten)

De verbinding met Key Vault bij het opstarten is ingesteld door een klasse die [Microsoft.AspNetCore.Hosting.IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup?view=aspnetcore-2.1) met behulp van een manier voor het uitbreiden van opstartgedrag dat wordt beschreven in [een app met een externe te verbeteren assembly in ASP.NET Core met IHostingStartup](/aspnet/core/fundamentals/host/platform-specific-configuration). De opstartklasse maakt gebruik van twee omgevingsvariabelen die de verbindingsgegevens van de Key Vault bevatten: ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONENABLED, ingesteld op true en ASPNETCORE_HOSTINGSTARTUP__KEYVAULT__CONFIGURATIONVAULT, ingesteld op uw sleutel URL van de kluis. Deze worden toegevoegd aan het bestand launchsettings.json wanneer u via uitvoert de **Add Connected Service** proces.

Voor toegang tot uw geheimen:

1. In Visual Studio in uw ASP.NET Core-project, u kunt nu verwijzen naar deze geheime gegevens met behulp van de volgende expressies in code:
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

1. Op een pagina .cshtml About.cshtml, zeg toevoegen de @inject richtlijn aan de bovenkant van het bestand voor het instellen van een variabele kunt u toegang tot de Key Vault-configuratie.

   ```cshtml
      @inject Microsoft.Extensions.Configuration.IConfiguration config
   ```

1. Als een test, kunt u bevestigen dat de waarde van de geheime sleutel beschikbaar is door deze op een van de pagina's wordt weergegeven. Gebruik @config om te verwijzen naar de config-variabele.
 
   ```cshtml
      <p> @config["MySecret"] </p>
      <p> @config.GetSection("Secrets")["MySecret"] </p>
      <p> @config["Secrets:MySecret"] </p>
   ```

1. Bouwen en uitvoeren van de web-App, gaat u naar de pagina over en de waarde 'geheim' zien.

## <a name="access-your-secrets-in-code-aspnet-471-projects"></a>Toegang tot uw geheimen in de code (ASP.NET 4.7.1 projecten)

De verbinding met uw Key Vault is ingesteld door de ConfigurationBuilder-klasse met behulp van informatie die is toegevoegd aan het bestand web.config, wanneer u via uitvoert de **Add Connected Service** proces.

Voor toegang tot uw geheimen:

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

Gefeliciteerd, u hebt nu bevestigd dat uw web-app Key Vault kunt gebruiken voor toegang tot geheimen veilig opgeslagen.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep. Hiermee verwijdert u de Key Vault en de gerelateerde resources. De resourcegroep verwijderen via de portal:

1. Voer de naam van uw resourcegroep in het vak Zoeken bovenaan de portal in. Wanneer u de in deze snelstart gebruikte resourcegroep in de zoekresultaten ziet, selecteert u deze.
2. Selecteer **Resourcegroep verwijderen**.
3. Typ in het vak **TYP DE NAAM VAN DE RESOURCEGROEP** de naam van de resourcegroep en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen van Key Vault vindt de [Ontwikkelaarshandleiding voor Key Vault](key-vault-developers-guide.md)
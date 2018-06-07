---
title: Ondersteuning voor Sleutelkluis toevoegen aan uw ASP.NET-project met Visual Studio | Microsoft Docs
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
ms.openlocfilehash: b4fed559b6364149170dc8b1da421c9c3ee1203c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34635760"
---
# <a name="add-key-vault-to-your-web-application-by-using-visual-studio-connected-services"></a>Sleutelkluis toevoegen aan uw webtoepassing met behulp van Visual Studio verbonden Services

In deze zelfstudie leert u hoe eenvoudig alles wat die u aan de slag met Azure Key Vault moet voor het beheren van uw geheimen voor webprojecten in Visual Studio, ongeacht of u ASP.NET Core of een ander type ASP.NET-project toevoegen. U kunt Visual Studio automatisch toevoegen van de NuGet-pakketten en configuratie-instellingen die u wilt verbinding maken met Sleutelkluis in Azure hebben via de functie verbonden Services in Visual Studio 2017. 

Zie voor meer informatie over de wijzigingen die zijn verbonden Services maakt in uw project om in te schakelen Sleutelkluis [Key Vault verbonden Service - wat is er gebeurd met mijn ASP.NET 4.7.1 project](vs-key-vault-aspnet-what-happened.md) of [Key Vault verbonden Service - wat is er gebeurd met Mijn project ASP.NET Core](vs-key-vault-aspnet-core-what-happened.md).

## <a name="prerequisites"></a>Vereisten

- **Een Azure-abonnement**. Als u nog geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).
- **Visual Studio 2017 versie 15,7** met de **webontwikkeling** werkbelasting geïnstalleerd. [Nu downloaden](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs).
- Voor ASP.NET-(geen Core) moet u .NET Framework 4.7.1 ontwikkelingsprogramma's, die niet standaard worden geïnstalleerd. Als u wilt installeren, start u de Visual Studio Installer, kiest u **wijzigen**, en kies vervolgens **afzonderlijke onderdelen**, vouw vervolgens aan de rechterkant **ASP.NET en web ontwikkeling**, en kies **.NET Framework 4.7.1 ontwikkelingsprogramma's**.
- Een ASP.NET 4.7.1 of ASP.NET Core 2.0-webproject openen.

## <a name="add-key-vault-support-to-your-project"></a>Ondersteuning voor Sleutelkluis toevoegen aan uw project

1. In **Solution Explorer**, kies **toevoegen** > **Connected-Service**.
   De pagina Connected-Service wordt weergegeven met services die u aan uw project toevoegen kunt.
1. Kies in het menu met beschikbare services **Secure geheimen met Azure Key Vault**.

   ![Kies 'Beveiligde geheimen met Azure Sleutelkluis'](media/vs-key-vault-add-connected-service/KeyVaultConnectedService1.PNG)

   Als u zich hebt aangemeld Visual Studio en een Azure-abonnement gekoppeld aan uw account hebt, verschijnt een pagina met een vervolgkeuzelijst met uw abonnementen.
1. Selecteer het abonnement dat u wilt gebruiken, en kies vervolgens een nieuwe of bestaande Sleutelkluis of kies de koppeling bewerken om de automatisch gegenereerde naam te wijzigen.

   ![Selecteer uw abonnement](media/vs-key-vault-add-connected-service/KeyVaultConnectedService3.PNG)

1. Typ de naam die u wilt gebruiken voor de Sleutelkluis.

   ![Wijzig de naam van de Sleutelkluis en een resourcegroep kiezen](media/vs-key-vault-add-connected-service/KeyVaultConnectedService-Edit.PNG)

1. Selecteer een bestaande resourcegroep of kies een nieuw bestand met de naam van een automatisch gegenereerde unqiue maken.  Als u een nieuwe groep maken met een andere naam wilt, kunt u de [Azure Portal](https://portal.azure.com), en de pagina te sluiten en opnieuw opstarten om opnieuw te laden van de lijst met resourcegroepen.
1. Kies de regio waarin u de Sleutelkluis te maken. Als uw webtoepassing wordt gehost in Azure, kiest u de regio die als host fungeert voor de webtoepassing voor optimale prestaties.
1. Kies een prijscategorie model. Zie voor meer informatie [Sleutelkluis prijzen](https://azure.microsoft.com/pricing/details/key-vault/).
1. Klik op OK om te accepteren van de gekozen configuratie.
1. Kies **toevoegen** voor het maken van de Sleutelkluis. Het aanmaakproces kan mislukken als u een naam die wordt al gebruikt.  Als dit gebeurt, gebruikt u de **bewerken** koppeling naar de naam van de Sleutelkluis en probeer het opnieuw.

   ![Verbonden service toevoegen aan project](media/vs-key-vault-add-connected-service/KeyVaultConnectedService4.PNG)

1. Voeg nu een geheim in uw Sleutelkluis in Azure. Als u op de juiste plaats in de portal, klik op de koppeling voor beheren geheimen die zijn opgeslagen in deze Sleutelkluis. Als u de pagina of het project hebt gesloten, u kunt naar navigeren in de [Azure-portal](https://portal.azure.com) door te kiezen **alle Services**onder **beveiliging**, kies **Sleutelkluis**, kiest u de Sleutelkluis die u zojuist hebt gemaakt.

   ![Navigeren naar de portal](media/vs-key-vault-add-connected-service/manage-secrets-link.jpg)

1. In de sectie Sleutelkluis voor de sleutel kluis hebt gemaakt, kiest u **geheimen**, klikt u vervolgens **Generate/importeren**.

   ![Een geheim genereren/importeren](media/vs-key-vault-add-connected-service/generate-secrets.jpg)

1. Voer een geheim, zoals "MySecret" en wijs hieraan de waarde van een tekenreeks als een test en kies vervolgens de **maken** knop.

   ![Een geheim maken](media/vs-key-vault-add-connected-service/create-a-secret.jpg)

1. (optioneel) Geef een andere geheim, maar deze tijd plaatsen in een categorie door de naam van 'Geheimen--MySecret'. Deze syntaxis Hiermee geeft u een categorie 'Geheimen' met een geheim 'MySecret'.
 
U kunt nu toegang tot uw geheimen in de code. De volgende stappen zijn verschillend, afhankelijk van of u van ASP.NET 4.7.1 of ASP.NET Core gebruikmaakt.

## <a name="access-your-secrets-in-code-aspnet-core-projects"></a>Toegang tot uw geheimen in de code (projecten ASP.NET Core)

1. In Visual Studio in uw project ASP.NET Core, u kunt nu verwijzen naar deze geheime gegevens met behulp van de volgende expressies in code:
 
   ```csharp
      config["MySecret"] // Access a secret without a section
      config["Secrets:MySecret"] // Access a secret in a section
      config.GetSection("Secrets")["MySecret"] // Get the configuration section and access a secret in it.
   ```

1. Op een pagina .cshtml spreken About.cshtml, voegt de @inject aan de bovenkant van het bestand voor het instellen van een variabele richtlijn kunt u toegang tot de Sleutelkluis-configuratie.

   ```cshtml
      @inject Microsoft.Extensions.Configuration.IConfiguration config
   ```

1. Als een test, kunt u bevestigen dat de waarde van het geheim beschikbaar is door deze op een van de pagina's weer te geven. Gebruik @config om te verwijzen naar de config-variabele.
 
   ```cshtml
      <p> @config["MySecret"] </p>
      <p> @config.GetSection("Secrets")["MySecret"] </p>
      <p> @config["Secrets:MySecret"] </p>
   ```

1. Bouwen en uitvoeren van de webtoepassing, navigeer naar de pagina over en de waarde 'geheim' ziet.

## <a name="access-your-secrets-in-code-aspnet-471-projects"></a>Toegang tot uw geheimen in de code (ASP.NET 4.7.1 projecten)

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

Gefeliciteerd, u nu hebt bevestigd dat uw web-app Sleutelkluis kunt gebruiken voor toegang tot geheimen veilig opgeslagen.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, verwijdert u de resourcegroep. Hiermee verwijdert u de Sleutelkluis en de bijbehorende resources. De resourcegroep verwijderen via de portal:

1. Voer de naam van uw resourcegroep in het vak Zoeken bovenaan de portal in. Wanneer u de in deze snelstart gebruikte resourcegroep in de zoekresultaten ziet, selecteert u deze.
2. Selecteer **Resourcegroep verwijderen**.
3. Typ in het vak **TYP DE NAAM VAN DE RESOURCEGROEP** de naam van de resourcegroep en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het ontwikkelen van Sleutelkluis door te lezen de [Sleutelkluis Developer Guide](key-vault-developers-guide.md)
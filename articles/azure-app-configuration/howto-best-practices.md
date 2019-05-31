---
title: Aanbevolen procedures voor Azure App-configuratie | Microsoft Docs
description: Leer hoe u optimaal gebruik van Azure App-configuratie
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: maiye
editor: ''
ms.assetid: ''
ms.service: azure-app-configuration
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: 3d9a597e7ced631627a121f3f0757e472f9a4bae
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393580"
---
# <a name="azure-app-configuration-best-practices"></a>Aanbevolen procedures voor Azure App-configuratie

Dit artikel worden algemene patronen en best practices, wanneer u Azure App-configuratie.

## <a name="key-groupings"></a>Belangrijkste groepen

App-configuratie biedt twee opties voor het indelen van sleutels:

* Belangrijkste voorvoegsels
* Labels

U kunt een of beide opties gebruiken voor het groeperen van uw sleutels.

*Sleutel voorvoegsels* zijn de begin-onderdelen van sleutels. U kunt een set sleutels logisch groeperen met behulp van hetzelfde voorvoegsel in hun namen. Voorvoegsels kunnen bevatten meerdere onderdelen die zijn verbonden met een scheidingsteken, zoals `/`, vergelijkbaar met een URL-pad, om te vormen een naamruimte. Dergelijke hiërarchieën zijn nuttig wanneer u sleutels voor veel toepassingen, componentservices en omgevingen in een configuratie van de App store opslaat.

Een belangrijke dingen die u moet rekening houden met zijn dat sleutels uw toepassingscode verwijst om op te halen van de waarden van de bijbehorende instellingen. Sleutels mag niet worden gewijzigd, anders hebt u uw code wijzigen telkens wanneer die plaatsvindt.

*Labels* zijn van een kenmerk voor sleutels. Deze worden gebruikt voor het maken van varianten van een sleutel. U kunt bijvoorbeeld labels toewijzen aan meerdere versies van een sleutel. Een versie is mogelijk een iteratie, een omgeving of andere contextuele informatie. Uw toepassing kan een geheel andere set met sleutelwaarden aanvragen door een ander label op te geven. Als gevolg hiervan blijven de verwijzingen van alle sleutels ongewijzigd in uw code.

## <a name="key-value-compositions"></a>Sleutel / waarde-samenstellingen

App-configuratie worden alle sleutels die ermee worden opgeslagen als onafhankelijke entiteiten behandeld. App-configuratie probeert niet afleiden van een relatie tussen sleutels of overnemen sleutelwaarden op basis van hun hiërarchie. U kunt echter meerdere sets met sleutels, samenvoegen door middel van labels in combinatie met de juiste configuratie stapelen in uw toepassingscode.

We bekijken een voorbeeld. Stel dat u hebt een instelling met de naam **Asset1**, waarvan de waarde kan variëren op basis van de ontwikkelomgeving. U maakt een sleutel met de naam 'Asset1' met een leeg label en een label met de naam 'Ontwikkeling'. In het eerste label, plaatst u de standaardwaarde voor **Asset1**, en u een specifieke waarde voor 'Ontwikkeling' in de laatste plaatsen.

Ophalen van de sleutelwaarden zonder de labels die in uw code, en vervolgens u dezelfde set sleutelwaarden een tweede keer met het label 'Ontwikkeling' ophalen. Wanneer u op de tweede keer dat de waarden ophaalt, worden de vorige waarden van de sleutels overschreven. Het configuratiesysteem .NET Core kunt u meerdere sets met configuratiegegevens boven op elkaar van 'stapeling'. Als een sleutel in meer dan één set bestaat, wordt het laatst ingestelde waarvan deze deel uitmaakt gebruikt. Met een moderne programmeerframework, zoals .NET Core, krijgt u deze mogelijkheid stapelen gratis als u een systeemeigen configuratieprovider gebruiken voor toegang tot App-configuratie. Het volgende codefragment laat zien hoe u stapelen in een .NET Core-toepassing kunt implementeren:

```csharp
// Augment the ConfigurationBuilder with Azure App Configuration
// Pull the connection string from an environment variable
configBuilder.AddAzureAppConfiguration(options => {
    options.Connect(configuration["connection_string"])
           .Use(KeyFilter.Any, LabelFilter.Null)
           .Use(KeyFilter.Any, "Development");
});
```

## <a name="app-configuration-bootstrap"></a>App-configuratie bootstrap

Voor toegang tot een App-configuratiearchief, kunt u de verbindingsreeks, die beschikbaar in de Azure-portal is. Omdat verbindingsreeksen referentie-informatie bevat, deze worden beschouwd als geheimen. Deze geheime gegevens moeten worden opgeslagen in Azure Key Vault en uw code moet worden geverifieerd met Key Vault om op te halen ze.

Een betere optie is het gebruik van de functie voor beheerde identiteiten in Azure Active Directory. U moet alleen de App-configuratie eindpunt-URL bootstrap toegang krijgen tot het opslaan van de App-configuratie met beheerde identiteiten. U kunt de URL in de code van uw toepassing insluiten (bijvoorbeeld in de *appsettings.json* bestand). Zie [integreren met Azure beheerde identiteiten](howto-integrate-azure-managed-service-identity.md) voor meer informatie.

## <a name="app-or-function-access-to-app-configuration"></a>App of functie toegang tot App-configuratie

U kunt toegang bieden aan App-configuratie voor web-apps of functies met behulp van de volgende methoden:

* Geef de verbindingsreeks voor het opslaan van de App-configuratie in de toepassingsinstellingen van App Service via de Azure-portal.
* De verbindingsreeks voor het opslaan van de configuratie van de App Store in Key Vault en [aanroepen vanuit App Service](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references).
* Gebruik Azure beheerde identiteiten die toegang tot de configuratie van de App store. Zie voor meer informatie, [integreren met Azure beheerde identiteiten](howto-integrate-azure-managed-service-identity.md).
* De pushconfiguratie van App-configuratie in App Service. App-configuratie biedt een exportfunctie (in Azure portal en de Azure CLI), waarmee gegevens rechtstreeks in App Service worden verzonden. Met deze methode hoeft u niet te wijzigen van de toepassingscode helemaal.

## <a name="next-steps"></a>Volgende stappen

* [Sleutels en waarden](./concept-key-value.md)

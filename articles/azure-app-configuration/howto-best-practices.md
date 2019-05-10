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
ms.openlocfilehash: d1275a48de5cad9321186ba20860d853b8ce55ad
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65413627"
---
# <a name="azure-app-configuration-best-practices"></a>Aanbevolen procedures voor Azure App-configuratie

Dit artikel worden algemene patronen en procedures bij het gebruik van Azure App-configuratie.

## <a name="key-groupings"></a>Belangrijkste groepen

App-configuratie biedt twee opties voor het indelen van sleutels: sleutel voorvoegsels of labels. U kunt een of beide.

Belangrijkste voorvoegsels zijn de begin-onderdelen van sleutels. U kunt een set sleutels logisch groeperen met behulp van hetzelfde voorvoegsel in hun namen. Voorvoegsels kunnen bevatten meerdere onderdelen met elkaar verbonden door een scheidingsteken zoals `/`, vergelijkbaar met een URL-pad, om te vormen een naamruimte. Dergelijke hiërarchieën zijn nuttig wanneer u bij het opslaan van sleutels voor veel toepassingen, componentservices en omgevingen in een configuratie van de App store. Een belangrijke dingen die u moet rekening houden met zijn dat sleutels uw toepassingscode verwijst om op te halen van de waarden van de bijbehorende instellingen. Een sleutel mag niet worden gewijzigd, omdat anders u moet code wijzigen telkens wanneer die plaatsvindt.

Labels is een kenmerk voor sleutels. Ze worden gebruikt voor het maken van varianten van een sleutel. U kunt bijvoorbeeld labels toewijzen aan meerdere versies van een sleutel. Een versie is een iteratie, omgeving of andere contextuele informatie. Uw toepassing kan een geheel andere set met waarden van sleutel aanvragen door een ander label op te geven. Alle verwijzingen van de sleutels kunnen blijven ongewijzigd.

## <a name="key-value-compositions"></a>Sleutel / waarde-samenstellingen

App-configuratie worden alle sleutels die ermee worden opgeslagen als onafhankelijke entiteiten behandeld. Het probeert niet afleiden van een relatie tussen sleutels of overnemen sleutelwaarden op basis van hun hiërarchie. U kunt meerdere sets met sleutels samenvoegen, echter met behulp van labels in combinatie met de juiste configuratie stapelen in uw toepassingscode.

We bekijken een voorbeeld. U hebt een instelling **Asset1** waarvan de waarde voor de omgeving 'Ontwikkeling' kan variëren. U kunt een sleutel met de naam 'Asset1' met een leeg label en een label met de naam 'Ontwikkeling' maken. U plaatst de standaardwaarde voor **Asset1** in de vorige en een specifieke waarde voor 'Ontwikkeling' in de laatste. In uw code, moet u eerst de sleutelwaarden zonder dat u een label en vervolgens die met het label 'Ontwikkeling' te overschrijven alle vorige waarden van de dezelfde sleutels ophalen. Als u een moderne programmeerframework zoals .NET Core gebruikt, kunt u deze mogelijkheid stapelen gratis ophalen als u een systeemeigen configuratieprovider gebruiken voor toegang tot App-configuratie. Het volgende codefragment toont hoe u kunt implementeren in een .NET Core-toepassing stapelen.

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

Voor toegang tot een App-configuratiearchief, kunt u de verbindingsreeks, die beschikbaar in de Azure-portal is. Tekenreeksen voor databaseverbindingen referentiegegevens bevatten en worden beschouwd als geheimen. Ze moeten worden opgeslagen in een Key Vault. Een betere optie is om Azure te gebruiken beheerde identiteit. Met deze methode hoeft u alleen App-configuratie eindpunt-URL naar de bootstrap-toegang tot het opslaan van de configuratie. U kunt de URL in de code van uw toepassing insluiten (bijvoorbeeld in de *appsettings.json* bestand). Zie [integreren met Azure beheerde identiteiten](howto-integrate-azure-managed-service-identity.md) voor meer informatie.

## <a name="web-app-or-function-access-to-app-configuration"></a>Web-App of functie toegang tot App-configuratie

U kunt de verbindingsreeks invoeren voor het opslaan van de App-configuratie in de toepassingsinstellingen van App Service via de Azure-portal. U kunt het ook opslaan in Key Vault en [aanroepen vanuit App Service](https://docs.microsoft.com/azure/app-service/app-service-key-vault-references). U kunt ook Azure beheerde identiteit voor toegang tot het opslaan van de configuratie. Zie [integreren met Azure beheerde identiteiten](howto-integrate-azure-managed-service-identity.md) voor meer informatie.

U kunt ook configuratie van App-configuratie pushen naar App Service. App-configuratie biedt een exportfunctie (in Azure portal en CLI), waarmee gegevens rechtstreeks in App Service worden verzonden. Met deze methode hoeft u niet helemaal wijzigen van de toepassingscode.

## <a name="next-steps"></a>Volgende stappen

* [Sleutels en waarden](./concept-key-value.md)

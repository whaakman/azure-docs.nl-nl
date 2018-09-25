---
title: Problemen oplossen met behulp van Azure blauwdrukken
description: Informatie over het oplossen van problemen met het maken en toewijzen van blauwdrukken
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: dd1163ece225c2e9a9b082f5e8364f34b06a10ae
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46982287"
---
# <a name="troubleshoot-errors-using-azure-blueprints"></a>Problemen oplossen met behulp van Azure blauwdrukken

Fouten bij het maken of toewijzen van blauwdrukken kunnen optreden. Dit artikel wordt beschreven voor verschillende fouten die zich kunnen voordoen en hoe u ze op te lossen.

## <a name="finding-error-details"></a>Foutdetails zoeken

Veel fouten is het resultaat van een blauwdruk toewijzen aan een bereik. Wanneer een toewijzing is mislukt, bevat blauwdruk informatie over de mislukte implementatie. Deze informatie wordt het probleem geven zodat deze kan worden opgelost en de volgende implementatie zal slagen.

1. Start de blauwdrukken voor Azure-service in Azure portal door te klikken op **alle services** en het zoeken en selecteren **beleid** in het linkerdeelvenster. Op de **beleid** pagina, klikt u op **blauwdrukken**.

1. Selecteer **toegewezen blauwdrukken** op de pagina aan de linkerkant en gebruik het zoekvak om te filteren van het blueprint-toewijzingen voor de mislukte-toewijzing niet vinden. U kunt ook de lijst met toewijzingen door sorteren de **Inrichtingsstatus** kolom om te zien van alle mislukte toewijzingen gegroepeerd.

1. Klik op de blauwdruk met de _mislukt_ status of met de rechtermuisknop en selecteer **Toewijzingsdetails weergeven**.

1. Aan de bovenkant van de blauwdruk is de toewijzing van pagina een rode banner waarschuwing dat de toewijzing is mislukt. Klik ergens op de koptekst voor meer informatie.

Het is gebruikelijk voor de fout wordt veroorzaakt door een artefact opgenomen in de blauwdruk en niet de blauwdruk als geheel. Bijvoorbeeld, als de blauwdruk bevat een artefact voor het maken van een Key Vault, maar het maken van Key Vault wordt voorkomen door Azure Policy, de volledige toewijzing mislukken.

## <a name="general-errors"></a>Algemene fouten

### <a name="policy-violation"></a>Scenario: Schending van het beleid

#### <a name="issue"></a>Probleem

De sjabloonimplementatie is mislukt vanwege schending van het beleid.

#### <a name="cause"></a>Oorzaak

Een beleid voor kan strijdig zijn met de implementatie voor een aantal oorzaken hebben:

- De resource wordt gemaakt, is beperkt door het beleid (meestal SKU of locatie beperkingen)
- De implementatie is bezig met velden die zijn geconfigureerd door het beleid (algemene met tags)

#### <a name="resolution"></a>Oplossing

De blauwdruk niet conflicteren met de beleidsregels die worden vermeld in de foutgegevens worden aangepast. Als dit niet mogelijk is, wordt een alternatieve opties is dat het bereik van de beleidstoewijzing gewijzigd, zodat de blauwdruk is niet langer een conflict met het beleid.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet wordt gezien of kan niet aan het oplossen van uw probleem, gaat u naar een van de volgende kanalen voor ondersteuning van meer:

- Krijg antwoorden van Azure-experts op [Azure-Forums](https://azure.microsoft.com/support/forums/)
- Maak verbinding met [@AzureSupport](https://twitter.com/azuresupport), het officiële Microsoft Azure-account voor het verbeteren van de gebruikerservaring door de Azure-community in contact te brengen met de juiste resources: antwoorden, ondersteuning en experts.
- Als u meer hulp nodig hebt, kunt u een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
---
title: Voorbeeld - SP NIST 800-53 R4 blauwdruk - stappen implementeren
description: Stappen voor het SP NIST 800-53 R4 blauwdruk voorbeeld implementeren.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 206763e2d17f4ad711ff5fd897f1429814e61837
ms.sourcegitcommit: 920ad23613a9504212aac2bfbd24a7c3de15d549
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "68228903"
---
# <a name="deploy-the-nist-sp-800-53-r4-blueprint-sample"></a>De SP NIST 800-53 R4 blauwdruk voorbeeld implementeren

Als u wilt de SP Azure blauwdrukken NIST 800-53 R4 blauwdruk voorbeeld implementeren, moeten de volgende stappen worden uitgevoerd:

> [!div class="checklist"]
> - Een nieuwe blauwdruk maken van de steekproef
> - Markeer uw kopie van het voorbeeld als **gepubliceerd**
> - Uw exemplaar van de blauwdruk toewijzen aan een bestaand abonnement

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="create-blueprint-from-sample"></a>Blauwdruk maken vanuit voorbeeld

Eerst de blauwdruk-voorbeeld implementeren door het maken van een nieuwe blauwdruk in uw omgeving met behulp van het voorbeeld als een starter.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Uit de **aan de slag** pagina aan de linkerkant, selecteer de **maken** knop onder _een blauwdruk maken_.

1. Zoek de **SP NIST 800-53 R4** blauwdruk voorbeeld onder _andere voorbeelden_ en selecteer **dit voorbeeld gebruiken**.

1. Voer de _basisbeginselen_ van de blauwdruk-voorbeeld:

   - **De blauwdruknaam van de**: Geef een naam voor uw exemplaar van het voorbeeld SP NIST 800-53 R4 blauwdruk.
   - **Definitielocatie**: Gebruik het beletselteken en de beheergroep om op te slaan, uw kopie van het voorbeeld te selecteren.

1. Selecteer de _artefacten_ tabblad aan de bovenkant van de pagina of **volgende: Artefacten** aan de onderkant van de pagina.

1. Bekijk de lijst met artefacten die gezamenlijk de blauwdruk-voorbeeld. Veel van de artefacten hebben parameters die Definieer later. Selecteer **concept opslaan** wanneer u klaar bent met het voorbeeld van de blauwdruk controleren.

## <a name="publish-the-sample-copy"></a>De voorbeeld-kopie publiceren

Nu is uw kopie van het voorbeeld van de blauwdruk gemaakt in uw omgeving. Deze gemaakt **Draft** modus en moet **gepubliceerd** voordat deze kunnen worden toegewezen en geïmplementeerd. De kopie van de blauwdruk voorbeeld kan worden aangepast aan uw omgeving en behoeften, maar deze wijziging van het uitlijning met SP NIST 800-53 besturingselementen afstappen kan.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om uw kopie van het voorbeeld van de blauwdruk zoeken en selecteer dit.

1. Selecteer **blauwdruk publiceren** aan de bovenkant van de pagina. In de nieuwe pagina aan de rechterkant, bieden een **versie** voor uw exemplaar van de blauwdruk-voorbeeld. Deze eigenschap is handig voor als u later een wijziging aanbrengt. Geef **notities wijzigen** zoals "eerste versie gepubliceerd vanuit de SP NIST 800-53 R4 blauwdruk voorbeeld." Selecteer vervolgens **publiceren** aan de onderkant van de pagina.

## <a name="assign-the-sample-copy"></a>De voorbeeld-kopie toewijzen

Nadat de kopie van het voorbeeld van de blauwdruk is **gepubliceerd**, deze kan worden toegewezen aan een abonnement in de beheergroep is opgeslagen op. Deze stap is waar de parameters worden opgegeven voor elke implementatie van de kopie van het voorbeeld van de blauwdruk uniek te maken.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om uw kopie van het voorbeeld van de blauwdruk zoeken en selecteer dit.

1. Selecteer **blauwdruk toewijzen** aan de bovenkant van de pagina van de blauwdruk-definitie.

1. Geef de parameterwaarden voor de blauwdruktoewijzing:

   - Basics

     - **Abonnementen**: Selecteer een of meer van de abonnementen die in de beheergroep die u zijn uw exemplaar van de blauwdruk monster opgeslagen. Als u meer dan één abonnement selecteert, wordt een toewijzing worden gemaakt voor elk gebruik van de ingevoerde parameters.
     - **Naam van de roltoewijzing**: De naam is ingevuld op basis van de naam van de blauwdruk.
       Indien nodig wijzigen in of laat is.
     - **Locatie**: Selecteer een regio voor de beheerde identiteit moet worden gemaakt. Azure Blueprint gebruikt deze beheerde identiteit om alle artefacten in de toegewezen blauwdruk te implementeren. Zie [Beheerde identiteiten voor Azure-resources](../../../../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie.
     - **De versie van blauwdruk**: Kies een **gepubliceerd** versie van uw exemplaar van de blauwdruk-voorbeeld.

   - LOCK-toewijzing

     Selecteer de blauwdruk vergrendeling instellen voor uw omgeving. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../../concepts/resource-locking.md).

   - Beheerde identiteit

     Laat de standaardwaarde _systeem toegewezen_ identiteitsoptie beheerd.

   - Artefact parameters

     De gedefinieerde parameters in deze sectie zijn van toepassing op het artefact waarin deze gedefinieerd. Deze parameters zijn [dynamische parameters](../../concepts/parameters.md#dynamic-parameters) omdat ze tijdens de toewijzing van de blauwdruk zijn gedefinieerd. Zie voor een volledige lijst of artefact parameters en de bijbehorende beschrijvingen, [artefact parameters tabel](#artifact-parameters-table).

1. Zodra alle parameters hebt ingevoerd, selecteert u **toewijzen** aan de onderkant van de pagina. De blauwdruktoewijzing is gemaakt en artefact-implementatie begint. De implementatie duurt ongeveer een uur. Als u wilt controleren op de status van implementatie, de blauwdruktoewijzing te openen.

> [!WARNING]
> De blauwdrukken voor Azure-service en de ingebouwde blauwdruk voorbeelden zijn **gratis**. Azure-resources zijn [prijs per product](https://azure.microsoft.com/pricing/). Gebruik de [prijscalculator](https://azure.microsoft.com/pricing/calculator/) om in te schatten van de kosten van het uitvoeren van de resources die zijn geïmplementeerd door deze blauwdruk-voorbeeld.

## <a name="artifact-parameters-table"></a>Tabel artefact-parameters

De volgende tabel geeft een lijst van de blauwdruk artefact parameters:

|Naam van het assemblyartefact|Artefacttype|Parameternaam|Description|
|-|-|-|-|
|\[Preview-versie\]: Controle van SP NIST 800-53 R4 besturingselementen en implementeren van specifieke VM-extensies ter ondersteuning van audit-vereisten|Beleidstoewijzing|Log Analytics-werkruimte-ID die virtuele machines moeten worden geconfigureerd voor|Dit is de ID (GUID) van de Log Analytics-werkruimte die de virtuele machines moeten worden geconfigureerd voor.|
|\[Preview-versie\]: Controle van SP NIST 800-53 R4 besturingselementen en implementeren van specifieke VM-extensies ter ondersteuning van audit-vereisten|Beleidstoewijzing|Lijst met resourcetypen die zouden moeten diagnostische logboeken zijn ingeschakeld zijn|Lijst met resourcetypen om te controleren als diagnostische logboeken instelling niet is ingeschakeld. Acceptabele waarden kunnen worden gevonden op [Azure Monitor diagnostische logboeken schema's](../../../../azure-monitor/platform/diagnostic-logs-schema.md#supported-log-categories-per-resource-type).|
|\[Preview-versie\]: Controle van SP NIST 800-53 R4 besturingselementen en implementeren van specifieke VM-extensies ter ondersteuning van audit-vereisten|Beleidstoewijzing|Lijst met gebruikers die moet worden uitgesloten van de groep Administrators van Windows-VM|Een door puntkomma's gescheiden lijst met leden die moeten worden uitgesloten van de lokale groep Administrators. Bijvoorbeeld: Beheerder. myUser1; myUser2|
|\[Preview-versie\]: Controle van SP NIST 800-53 R4 besturingselementen en implementeren van specifieke VM-extensies ter ondersteuning van audit-vereisten|Beleidstoewijzing|Lijst met gebruikers die moeten worden opgenomen in de groep Administrators van Windows-VM|Een door puntkomma's gescheiden lijst met leden die moeten worden opgenomen in de lokale groep Administrators. Bijvoorbeeld: Beheerder. myUser1; myUser2|
|\[Preview-versie\]: Log Analytics-Agent voor Linux VM-Schaalsets (VMSS) implementeren|Beleidstoewijzing|Log Analytics-werkruimte voor Linux VM Scale Sets (VMSS)|Als deze werkruimte buiten het bereik van de toewijzing is moet u handmatig 'Inzender van Log Analytics'-machtigingen verlenen (of vergelijkbaar) in de beleidstoewijzing principal-ID.|
|\[Preview-versie\]: Log Analytics-Agent voor Linux VM-Schaalsets (VMSS) implementeren|Beleidstoewijzing|Optioneel: Lijst met VM-installatiekopieën waarop ondersteunde Linux-besturingssysteem toe te voegen aan het bereik|Een lege matrix kan worden gebruikt om aan te geven zonder optionele parameters: \[\]|
|\[Preview-versie\]: Log Analytics-Agent voor Linux-VM's implementeren|Beleidstoewijzing|Log Analytics-werkruimte voor virtuele Linux-machines|Als deze werkruimte buiten het bereik van de toewijzing is moet u handmatig 'Inzender van Log Analytics'-machtigingen verlenen (of vergelijkbaar) in de beleidstoewijzing principal-ID.|
|\[Preview-versie\]: Log Analytics-Agent voor Linux-VM's implementeren|Beleidstoewijzing|Optioneel: Lijst met VM-installatiekopieën waarop ondersteunde Linux-besturingssysteem toe te voegen aan het bereik|Een lege matrix kan worden gebruikt om aan te geven zonder optionele parameters: \[\]|
|\[Preview-versie\]: Log Analytics-Agent voor Windows VM-Schaalsets (VMSS) implementeren|Beleidstoewijzing|Log Analytics-werkruimte voor Windows VM Scale Sets (VMSS)|Als deze werkruimte buiten het bereik van de toewijzing is moet u handmatig 'Inzender van Log Analytics'-machtigingen verlenen (of vergelijkbaar) in de beleidstoewijzing principal-ID.|
|\[Preview-versie\]: Log Analytics-Agent voor Windows VM-Schaalsets (VMSS) implementeren|Beleidstoewijzing|Optioneel: Lijst met VM-installatiekopieën waarop ondersteunde Windows-besturingssysteem toe te voegen aan het bereik|Een lege matrix kan worden gebruikt om aan te geven zonder optionele parameters: \[\]|
|\[Preview-versie\]: Log Analytics-Agent voor Windows-VM's implementeren|Beleidstoewijzing|Log Analytics-werkruimte voor Windows-VM 's|Als deze werkruimte buiten het bereik van de toewijzing is moet u handmatig 'Inzender van Log Analytics'-machtigingen verlenen (of vergelijkbaar) in de beleidstoewijzing principal-ID.|
|\[Preview-versie\]: Log Analytics-Agent voor Windows-VM's implementeren|Beleidstoewijzing|Optioneel: Lijst met VM-installatiekopieën waarop ondersteunde Windows-besturingssysteem toe te voegen aan het bereik|Een lege matrix kan worden gebruikt om aan te geven zonder optionele parameters: \[\]|
|Advanced Threat Protection op Storage-Accounts te implementeren|Beleidstoewijzing|Effect|Informatie over de gevolgen van beleid kan worden gevonden op [effecten van Azure-beleid begrijpen](../../../policy/concepts/effects.md)|
|Controle op SQL-servers implementeren|Beleidstoewijzing|De waarde in dagen van de bewaarperiode (0 geeft een onbeperkte bewaarperiode)|Logboekbehoud in dagen (optioneel, 180 dagen als u niets opgeeft)|
|Controle op SQL-servers implementeren|Beleidstoewijzing|Resourcegroepnaam voor storage-account voor SQL server-controle|Meld u controle schrijfbewerkingen databasegebeurtenissen naar een auditlogboek in uw Azure Storage-account (een storage-account wordt gemaakt in elke regio waarin een SQL-Server wordt gemaakt die worden gedeeld door alle servers in die regio). Belangrijk - voor een goede werking van de controle niet verwijderd of wijzig de naam van de resourcegroep of de storage-accounts.|
|Diagnostische instellingen voor Netwerkbeveiligingsgroepen implementeren|Beleidstoewijzing|Opslagaccountvoorvoegsel voor network security group diagnostische gegevens|Dit voorvoegsel wordt gecombineerd met de beveiliging groep locatie om te vormen van de naam van het gemaakte opslagaccount.|
|Diagnostische instellingen voor Netwerkbeveiligingsgroepen implementeren|Beleidstoewijzing|Resourcegroepnaam voor opslagaccount voor diagnostische gegevens network security group (moet al bestaan)|De resourcegroep waarin het storage-account wordt gemaakt. Deze resourcegroep moet al bestaan.|

## <a name="next-steps"></a>Volgende stappen

Nu dat u de stappen voor het implementeren van de SP NIST 800-53 R4 blauwdruk voorbeeld hebt bekeken, gaat u naar de volgende artikelen voor meer informatie over de blauwdruk en de toewijzing van het besturingselement:

> [!div class="nextstepaction"]
> [SP NIST 800-53 R4 blauwdruk - overzicht](./index.md)
> [SP NIST 800-53 R4 blauwdruk - besturingselement toewijzing](./control-mapping.md)

Aanvullende artikelen over blauwdrukken en het gebruik hiervan:

- Meer informatie over de [levenscyclus van een blauwdruk](../../concepts/lifecycle.md).
- Meer informatie over hoe u [statische en dynamische parameters](../../concepts/parameters.md) gebruikt.
- Meer informatie over hoe u de [blauwdrukvolgorde](../../concepts/sequencing-order.md) aanpast.
- Meer informatie over hoe u gebruikmaakt van [resourcevergrendeling in blauwdrukken](../../concepts/resource-locking.md).
- Meer informatie over hoe u [bestaande toewijzingen bijwerkt](../../how-to/update-existing-assignments.md).

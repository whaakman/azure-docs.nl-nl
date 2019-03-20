---
title: Voorbeeld - ISO 27001 gedeelde Services blauwdruk - stappen implementeren
description: Stappen van het voorbeeld van de blauwdruk ISO 27001 gedeelde Services implementeren.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 64259d029ce67ecfd5db4b97fc165be0ee391ab8
ms.sourcegitcommit: 4133f375862fdbdec07b70de047d70c66ac29d50
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/15/2019
ms.locfileid: "58008078"
---
# <a name="deploy-the-azure-blueprints-iso-27001-shared-services-blueprint-sample"></a>Het voorbeeld van de blauwdruk Azure blauwdrukken ISO 27001 gedeelde Services implementeren

Voor het implementeren van het voorbeeld van de blauwdruk Azure blauwdrukken ISO 27001 gedeelde Services, moeten de volgende stappen worden uitgevoerd:

> [!div class="checklist"]
> - Een nieuwe blauwdruk maken van de steekproef
> - Markeer uw kopie van het voorbeeld als **gepubliceerd**
> - Uw exemplaar van de blauwdruk toewijzen aan een bestaand abonnement

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free) aan voordat u begint.

## <a name="create-blueprint-from-sample"></a>Blauwdruk maken vanuit voorbeeld

Eerst de blauwdruk-voorbeeld implementeren door het maken van een nieuwe blauwdruk in uw omgeving met behulp van het voorbeeld als een starter.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Uit de **aan de slag** pagina aan de linkerkant, selecteer de **maken** knop onder _een blauwdruk maken_.

1. Zoek de **ISO 27001: Gedeelde Services** blauwdruk voorbeeld onder _andere voorbeelden_ en selecteer **dit voorbeeld gebruiken**.

1. Voer de _basisbeginselen_ van de blauwdruk-voorbeeld:

   - **De blauwdruknaam van de**: Geef een naam voor uw exemplaar van het voorbeeld van de blauwdruk ISO 27001 gedeelde Services.
   - **Definitielocatie**: Gebruik het beletselteken en de beheergroep om op te slaan, uw kopie van het voorbeeld te selecteren.

1. Selecteer de _artefacten_ tabblad aan de bovenkant van de pagina of **volgende: Artefacten** aan de onderkant van de pagina.

1. Bekijk de lijst met artefacten die gezamenlijk de blauwdruk-voorbeeld. Veel van de artefacten hebben parameters die Definieer later. Selecteer **concept opslaan** wanneer u klaar bent met het voorbeeld van de blauwdruk controleren.

## <a name="publish-the-sample-copy"></a>De voorbeeld-kopie publiceren

Nu is uw kopie van het voorbeeld van de blauwdruk gemaakt in uw omgeving. Deze gemaakt **Draft** modus en moet **gepubliceerd** voordat deze kunnen worden toegewezen en geïmplementeerd. De kopie van de blauwdruk voorbeeld kan worden aangepast aan uw omgeving en behoeften, maar deze wijziging van de ISO 27001-standaard verplaatsen kan.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om uw kopie van het voorbeeld van de blauwdruk zoeken en selecteer dit.

1. Selecteer **blauwdruk publiceren** aan de bovenkant van de pagina. In de nieuwe pagina aan de rechterkant, bieden een **versie** voor uw exemplaar van de blauwdruk-voorbeeld. Deze eigenschap is handig voor als u later een wijziging aanbrengt. Geef **notities wijzigen** zoals "eerste versie gepubliceerd vanuit de voorbeeld-ISO 27001 blauwdruk." Selecteer vervolgens **publiceren** aan de onderkant van de pagina.

## <a name="assign-the-sample-copy"></a>De voorbeeld-kopie toewijzen

Nadat de kopie van het voorbeeld van de blauwdruk is **gepubliceerd**, deze kan worden toegewezen aan een abonnement in de beheergroep is opgeslagen op. Deze stap is waar de parameters worden opgegeven voor elke implementatie van de kopie van het voorbeeld van de blauwdruk uniek te maken.

1. Selecteer **alle services** en zoek en selecteer **beleid** in het linkerdeelvenster. Op de **beleid** weergeeft, schakelt **blauwdrukken**.

1. Selecteer de **blauwdruk definities** pagina aan de linkerkant. De filters gebruiken om uw kopie van het voorbeeld van de blauwdruk zoeken en selecteer dit.

1. Selecteer **blauwdruk toewijzen** aan de bovenkant van de pagina van de blauwdruk-definitie.

1. Geef de parameterwaarden voor de blauwdruktoewijzing:

   - Basisbeginselen

     - **Abonnementen**: Selecteer een of meer van de abonnementen die in de beheergroep die u zijn uw exemplaar van de blauwdruk monster opgeslagen. Als u meer dan één abonnement selecteert, wordt een toewijzing worden gemaakt voor elk gebruik van de ingevoerde parameters.
     - **Naam van de roltoewijzing**: De naam is ingevuld op basis van de naam van de blauwdruk.
       Indien nodig wijzigen in of laat is.
     - **Locatie**: Selecteer een regio voor de beheerde identiteit moet worden gemaakt. Azure Blueprint gebruikt deze beheerde identiteit om alle artefacten in de toegewezen blauwdruk te implementeren. Zie [Beheerde identiteiten voor Azure-resources](../../../../active-directory/managed-identities-azure-resources/overview.md) voor meer informatie.
     - **De versie van blauwdruk**: Kies een **gepubliceerd** versie van uw exemplaar van de blauwdruk-voorbeeld.

   - Toewijzing vergrendelen

     Selecteer de blauwdruk vergrendeling instellen voor uw omgeving. Zie voor meer informatie [Vergrendeling van blauwdrukresources](../../concepts/resource-locking.md).

   - Beheerde identiteit

     Laat de standaardwaarde _systeem toegewezen_ identiteitsoptie beheerd.

   - Blauwdrukparameters

     De gedefinieerde parameters in deze sectie worden gebruikt door veel van de artefacten in het blauwdrukdefinitie van de consistentie kunt bieden.

     - **Naam van de organisatie**: Geef een korte-naam voor uw organisatie. Deze eigenschap wordt voornamelijk gebruikt voor de naamgeving van resources.
     - **Gedeelde services subnet adresvoorvoegsel**: Geef de CIDR-notatiewaarde voor de geïmplementeerde resources samen netwerken.
     - **Gedeelde services locatie**: Hiermee bepaalt u welke locatie de artefacten zijn geïmplementeerd op. Niet alle services zijn beschikbaar in alle locaties. Artefacten implementeren van deze services bieden een parameteroptie voor voor de locatie die artefact dat moet implementeren.
     - **Locatie toegestaan (beleid: Blauwdruk initiatief voor ISO 27001)**: De waarde die aangeeft van de toegestane locaties voor resourcegroepen en resources.
     - **Log Analytics-werkruimte voor VM-agents (beleid: Blauwdruk initiatief voor ISO 27001)**: Hiermee geeft u de Resource-ID van een werkruimte. Deze parameter gebruikt een `concat` functie voor het maken van de Resource-ID.

   - Artefactparameters

     De gedefinieerde parameters in deze sectie zijn van toepassing op het artefact waarin deze gedefinieerd. Deze parameters zijn [dynamische parameters](../../concepts/parameters.md#dynamic-parameters) omdat ze tijdens de toewijzing van de blauwdruk zijn gedefinieerd. Zie voor een volledige lijst of artefact parameters en de bijbehorende beschrijvingen, [artefact parameters tabel](#artifact-parameters-table).

1. Zodra alle parameters hebt ingevoerd, selecteert u **toewijzen** aan de onderkant van de pagina.

> [!WARNING]
> De blauwdrukken voor Azure-service en de ingebouwde blauwdruk voorbeelden zijn **gratis**. Azure-resources zijn [prijs per product](https://azure.microsoft.com/en-us/pricing/). Gebruik de [prijscalculator](https://azure.microsoft.com/pricing/calculator/) om in te schatten van de kosten van het uitvoeren van de resources die zijn geïmplementeerd door deze blauwdruk-voorbeeld.

## <a name="artifact-parameters-table"></a>Tabel artefact-parameters

De volgende tabel geeft een lijst van de blauwdruk artefact parameters:

|Naam van het artefact|Type artefact|Parameternaam|Description|
|-|-|-|-|
|[Preview]: Log Analytics-Agent voor Linux VM-Schaalsets (VMSS) implementeren|Beleidstoewijzing|Optioneel: Lijst met VM-installatiekopieën waarop ondersteunde Linux-besturingssysteem toe te voegen aan het bereik|(Optioneel) Standaardwaarde is _['geen']_.|
|[Preview]: Log Analytics-Agent voor Linux-VM's implementeren|Beleidstoewijzing|Optioneel: Lijst met VM-installatiekopieën waarop ondersteunde Linux-besturingssysteem toe te voegen aan het bereik|(Optioneel) Standaardwaarde is _['geen']_.|
|[Preview]: Log Analytics-Agent voor Windows VM-Schaalsets (VMSS) implementeren|Beleidstoewijzing|Optioneel: Lijst met VM-installatiekopieën waarop ondersteunde Windows-besturingssysteem toe te voegen aan het bereik|(Optioneel) Standaardwaarde is _['geen']_.|
|[Preview]: Log Analytics-Agent voor Windows-VM's implementeren|Beleidstoewijzing|Optioneel: Lijst met VM-installatiekopieën waarop ondersteunde Windows-besturingssysteem toe te voegen aan het bereik|(Optioneel) Standaardwaarde is _['geen']_.|
|Toegestane brontypen|Beleidstoewijzing|Toegestane brontypen|Lijst met resourcetypen die mogen worden geïmplementeerd. Deze lijst bestaat uit alle brontypen die zijn geïmplementeerd in gedeelde Services.|
|Toegestane opslagaccount-SKU's|Beleidstoewijzing|Toegestane SKU's voor opslag|Lijst met diagnostische logboeken toegestane SKU's voor opslag. Standaardwaarde is _["Standard_LRS"]_.|
|Toegestane SKU's van virtuele machines|Beleidstoewijzing|Lijst met SKU's voor virtuele machine mag worden geïmplementeerd. Standaardwaarde is _['Standard_DS1_v2', "Standard_DS2_v2"]_.|
|Blauwdrukinitiatief voor ISO 27001|Beleidstoewijzing|Resourcetypen om te controleren, logboeken met diagnostische gegevens|Lijst met resourcetypen om te controleren als diagnostische logboeken instelling niet is ingeschakeld. Acceptabele waarden kunnen worden gevonden op [Azure Monitor diagnostische logboeken schema's](../../../../azure-monitor/platform/diagnostic-logs-schema.md#supported-log-categories-per-resource-type).|
|Log Analytics-resourcegroep|Resourcegroep|Name|**Vergrendeld** -worden samengevoegd in de **organisatienaam** met `-sharedsvsc-log-rg` uniek te maken de resourcegroep.|
|Log Analytics-resourcegroep|Resourcegroep|Locatie|**Vergrendeld** -de blauwdrukparameter wordt gebruikt.|
|Log Analytics-sjabloon|Resource Manager-sjabloon|Servicelaag|Hiermee stelt u de laag van de Log Analytics-werkruimte. Standaardwaarde is _PerNode_.|
|Log Analytics-sjabloon|Resource Manager-sjabloon|Behoud van logboeken in dagen|Het bewaren van gegevens in dagen. Standaardwaarde is _365_.|
|Log Analytics-sjabloon|Resource Manager-sjabloon|Locatie|De regio die wordt gebruikt voor het maken van de Log Analytics-werkruimte. Standaardwaarde is _VS-West 2_.|
|Netwerk-resourcegroep|Resourcegroep|Name|**Vergrendeld** -worden samengevoegd in de **organisatienaam** met `-sharedsvcs-net-rg` uniek te maken de resourcegroep.|
|Netwerk-resourcegroep|Resourcegroep|Locatie|**Vergrendeld** -de blauwdrukparameter wordt gebruikt.|
|Sjabloon voor Azure Firewall|Resource Manager-sjabloon|Privé-IP-adres voor Azure-firewall|Hiermee configureert u de privé IP-adres van de [Azure firewall](../../../../firewall/overview.md). Deze waarde wordt ook gebruikt als de standaardrouteringstabel op gedeelde services subnet. Moeten deel uitmaken van de CIDR-notatie die is gedefinieerd in **Azure Firewall subnet adresvoorvoegsel**. Standaardwaarde is _10.0.4.4_.|
|Sjabloon voor Azure Firewall|Resource Manager-sjabloon|Behoud van logboeken in dagen|Het bewaren van gegevens in dagen. Standaardwaarde is _365_.|
|Sjabloon voor netwerkbeveiligingsgroep|Resource Manager-sjabloon|Behoud van logboeken in dagen|Het bewaren van gegevens in dagen. Standaardwaarde is _365_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|Voorvoegsel van adres voor virtueel netwerk|De CIDR-notatie voor het virtuele netwerk. Standaardwaarde is _10.0.0.0/16_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|DDoS-beveiliging voor virtueel netwerk inschakelen|Hiermee configureert u een DDoS-bescherming voor het virtuele netwerk. Standaardwaarde is _waar_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|Gedeelde Services subnet adresvoorvoegsel|De CIDR-notatie voor het subnet voor gedeelde Services. Standaardwaarde is _10.0.0.0/24_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|Voorvoegsel van subnetadres van DMZ|De CIDR-notatie voor het DMZ-subnet. Standaardwaarde is _10.0.1.0/24_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|Voorvoegsel van subnetadres van Application Gateway|De CIDR-notatie voor het subnet voor de toepassingsgateway. Standaardwaarde is _10.0.2.0/24_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|Voorvoegsel van subnetadres van gateway voor virtueel netwerk|De CIDR-notatie voor het gatewaysubnet voor het virtuele netwerk. Standaardwaarde is _10.0.3.0/24_.|
|Sjabloon voor virtueel netwerk en routetabel|Resource Manager-sjabloon|Voorvoegsel van subnetadres van Azure Firewall|De CIDR-notatie voor de [Azure firewall](../../../../firewall/overview.md) subnet. Bevatten de **firewall van Azure privé IP-adres** parameter.|
|Key Vault-resourcegroep|Resourcegroep|Name|**Vergrendeld** -worden samengevoegd in de **organisatienaam** met `-sharedsvcs-kv-rg` uniek te maken de resourcegroep.|
|Key Vault-resourcegroep|Resourcegroep|Locatie|**Vergrendeld** -de blauwdrukparameter wordt gebruikt.|
|Key Vault-sjabloon|Resource Manager-sjabloon|Gebruikersnaam van Jumpbox-beheerder|Gebruikersnaam voor de jumpbox. Moet overeenkomen met dezelfde waarde voor eigenschap in **Jumpbox sjabloon**. Standaardwaarde is _jb-admin-gebruiker_.|
|Key Vault-sjabloon|Resource Manager-sjabloon|SSH-sleutel of wachtwoord van de Jumpbox- beheerder|Sleutel of het wachtwoord voor het account op de jumpbox. Moet overeenkomen met dezelfde waarde voor eigenschap in **Jumpbox sjabloon**. Er is geen standaard waarde en mag niet leeg.|
|Key Vault-sjabloon|Resource Manager-sjabloon|Gebruikersnaam van domeinbeheerder|De gebruikersnaam die wordt gebruikt voor toegang tot Active Directory-VM en andere VM's toevoegen aan een domein. Moet overeenkomen met **domeingebruiker met beheerdersrechten** eigenschapswaarde in **Active Directory Domain Services-sjabloon**. Standaardwaarde is _domein-admin-gebruiker_.|
|Key Vault-sjabloon|Resource Manager-sjabloon|Wachtwoord van domeinbeheerder|Domein-beheerder het wachtwoord van gebruiker. Er is geen standaard waarde en mag niet leeg.|
|Key Vault-sjabloon|Resource Manager-sjabloon|AAD-object-id|De AAD-object-id van het account dat toegang tot de Key Vault-instantie is vereist. Er is geen standaard waarde en mag niet leeg. U vindt deze waarde uit de Azure portal door zoek en selecteer 'Gebruikers' onder _Services_. Gebruik de _naam_ in om te filteren op naam van de account en selecteert u dat account. Op de _gebruikersprofiel_ pagina, selecteer het pictogram 'Klikken om te kopiëren' bij de _Object-ID_.  |
|Key Vault-sjabloon|Resource Manager-sjabloon|Behoud van logboeken in dagen|Het bewaren van gegevens in dagen. Standaardwaarde is _365_.|
|Key Vault-sjabloon|Resource Manager-sjabloon|Key Vault-SKU|Hiermee geeft u de SKU van de Sleutelkluis die is gemaakt. Standaardwaarde is _Premium_.|
|Jumpbox-resourcegroep|Resourcegroep|Name|**Vergrendeld** -worden samengevoegd in de **organisatienaam** met `-sharedsvcs-jb-rg` uniek te maken de resourcegroep.|
|Jumpbox-resourcegroep|Resourcegroep|Locatie|**Vergrendeld** -de blauwdrukparameter wordt gebruikt.|
|Jumpbox-sjabloon|Resource Manager-sjabloon|Gebruikersnaam van Jumpbox-beheerder|De gebruikersnaam die wordt gebruikt voor toegang tot de jumpbox-VM's. Moet overeenkomen met dezelfde waarde voor eigenschap in **Key Vault sjabloon**. Standaardwaarde is _jb-admin-gebruiker_.|
|Jumpbox-sjabloon|Resource Manager-sjabloon|Het beheerderswachtwoord Jumpbox (Key Vault Resource-ID)|De Resource-ID van de Key Vault. Gebruik '/ subscriptions/{subscriptionId}/resourceGroups/{orgName}-sharedsvcs-kv-rg/providers/Microsoft.KeyVault/vaults/{orgName}-sharedsvcs-kv' en vervang `{subscriptionId}` door uw abonnements-ID en `{orgName}` met de  **Naam van de organisatie** blauwdruk parameter.|
|Jumpbox-sjabloon|Resource Manager-sjabloon|Het beheerderswachtwoord Jumpbox (Key Vault-geheim naam)|Gebruikersnaam van de jumpbox-beheerder. Moet overeenkomen met de waarde in **Key Vault sjabloon** eigenschap **Jumpbox beheerdersgebruikersnaam**.|
|Jumpbox-sjabloon|Resource Manager-sjabloon|Jumpbox-besturingssysteem|Hiermee bepaalt u het besturingssysteem van de jumpbox-VM. Standaardwaarde is _Windows_.|
|Active Directory Domain Services-resourcegroep|Resourcegroep|Name|**Vergrendeld** -worden samengevoegd in de **organisatienaam** met `-sharedsvcs-adds-rg` uniek te maken de resourcegroep.|
|Active Directory Domain Services-resourcegroep|Resourcegroep|Locatie|**Vergrendeld** -de blauwdrukparameter wordt gebruikt.|
|Sjabloon voor Active Directory Domain Services|Resource Manager-sjabloon|Gebruikersnaam van domeinbeheerder|Gebruikersnaam voor de jumpbox toevoegen. Moet overeenkomen met dezelfde waarde voor eigenschap in **Key Vault sjabloon**. Standaardwaarde is _voegt-admin-gebruiker_.|
|Sjabloon voor Active Directory Domain Services|Resource Manager-sjabloon|Het beheerderswachtwoord domein (Key Vault Resource-ID)|De Resource-ID van de Key Vault. Gebruik '/ subscriptions/{subscriptionId}/resourceGroups/{orgName}-sharedsvcs-kv-rg/providers/Microsoft.KeyVault/vaults/{orgName}-sharedsvcs-kv' en vervang `{subscriptionId}` door uw abonnements-ID en `{orgName}` met de  **Naam van de organisatie** blauwdruk parameter.|
|Sjabloon voor Active Directory Domain Services|Resource Manager-sjabloon|Het beheerderswachtwoord domein (Key Vault-geheim naam)|Gebruikersnaam van de domein-beheerder. Moet overeenkomen met de waarde in **Key Vault sjabloon** eigenschap **admin domeingebruikersnaam**.|
|Sjabloon voor Active Directory Domain Services|Resource Manager-sjabloon|Domeinnaam|De naam van de Active Directory die zijn gemaakt door het voorbeeld. Standaardwaarde is _contoso.com_.|
|Sjabloon voor Active Directory Domain Services|Resource Manager-sjabloon|Gebruiker met beheerdersrechten|Gebruikersnaam voor het beheerdersaccount van AD en voor apparaten toevoegen aan het AD-domein. Moet overeenkomen met **gebruikersnaam van de AD-beheerder** eigenschapswaarde in **Key Vault sjabloon**. Standaardwaarde is _domein-admin-gebruiker_.|
|Sjabloon voor Active Directory Domain Services|Resource Manager-sjabloon|Wachtwoord van domeinbeheerder|De details van de Key Vault voor het opslaan van het wachtwoord instellen. Er is geen standaard waarde en mag niet leeg.|

## <a name="next-steps"></a>Volgende stappen

Nu dat u de stappen voor het implementeren van het voorbeeld van de blauwdruk ISO 27001 gedeelde Services hebt doorgenomen, gaat u naar de volgende artikelen voor meer informatie over de architectuur en het besturingselement toewijzing:

> [!div class="nextstepaction"]
> [ISO 27001 gedeelde Services blauwdruk - overzicht](./index.md)
> [ISO 27001 gedeelde Services blauwdruk - besturingselement toewijzing](./control-mapping.md)

Toevoeging artikelen over blauwdrukken en hoe ze te gebruiken:

- Meer informatie over de [blauwdruk levenscyclus](../../concepts/lifecycle.md).
- Meer informatie over het gebruik van [statische en dynamische parameters](../../concepts/parameters.md).
- Meer informatie over het aanpassen van de [blauwdruk volgorde](../../concepts/sequencing-order.md).
- Ontdek hoe u het gebruik van [blauwdruk resource vergrendelen](../../concepts/resource-locking.md).
- Meer informatie over het [bijwerken, bestaande toewijzingen](../../how-to/update-existing-assignments.md).
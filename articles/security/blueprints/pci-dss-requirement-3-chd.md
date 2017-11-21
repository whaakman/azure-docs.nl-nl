---
title: Azure betaling verwerking blauwdruk - CHD vereisten
description: PCI-DSS vereiste 3
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 9736f7c8-c632-4b86-8b8a-6e4f45c1a7aa
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 356599cbe1e4e1948a5ec16d0d504835fa7dcd43
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="chd-requirements-for-pci-dss-compliant-environments"></a>CHD vereisten voor PCI DSS-compatibele omgevingen
## <a name="pci-dss-requirement-3"></a>PCI-DSS vereiste 3

**Opgeslagen kaarthouder gegevens beveiligen**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Beveiligingsmethoden zoals versleuteling, moet worden afgekapt, maskeren, en belangrijke onderdelen van de kaarthouder gegevensbeveiliging hashing zijn. Als een indringer het gevolg van andere beveiligingsmechanismen en toegang te tot de versleutelde gegevens, zonder de juiste cryptografische sleutels krijgen worden de gegevens zijn onleesbaar en onbruikbaar worden voor deze persoon. Andere effectieve methoden voor het beveiligen van opgeslagen gegevens moeten ook worden overwogen als mogelijke risico risicobeperking verkoopkansen. Methoden voor het risico minimaliseren bevatten bijvoorbeeld niet Sla kaarthouder-gegevens tenzij dit echt nodig, Bezig met afbreken kaarthouder gegevens als volledige PANNEN is niet nodig en niet verzenden onbeveiligde pannen met behulp van de eindgebruiker-messaging-technologieën, zoals e-mail en chatberichten Messaging.
Raadpleeg de PCI DSS en PA-DSS verklarende woordenlijst van termen, afkortingen en acroniemen voor definities van 'sterke cryptografie' en andere voorwaarden PCI DSS.

## <a name="pci-dss-requirement-31"></a>PCI-DSS vereiste 3.1

**3.1** behouden kaarthouder gegevensopslag tot een minimum beperkt door het implementeren van beleid voor het bewaren en verwijdering van gegevens, procedures en processen die ten minste de volgende voor alle kaarthouder voor de opslag van gegevens (CHD zijn):
- Beperken van de gegevens opslag bedrag en retentie tijd die is vereist voor juridische, regelgevende en zakelijke vereisten
- Vereisten voor de specifieke bewaarperiode voor gegevens van de kaarthouder
- Processen voor het veilig verwijderen van gegevens wanneer deze niet langer nodig is
- Een driemaandelijkse proces voor het identificeren en veilig verwijderen van opgeslagen gegevens kaarthouder die langer is dan de opgegeven bewaarperiode.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Azure is verantwoordelijk voor de klantgegevens die is aangewezen voor verwijdering worden veilig buiten gebruik gestelde 800 88 compatibele protocollen die in het beleid van de verwijdering beveiligen met behulp van NIST. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore niet verwijderen of een opgeslagen CHD vernietigen. Maar alle gegevens zijn versleuteld en geen primaire account nummer (PAN) gegevens worden opgeslagen.<br /><br />|



## <a name="pci-dss-requirement-32"></a>PCI-DSS vereiste 3.2

**3.2** Sla gevoelige verificatiegegevens niet na autorisatie (zelfs als versleuteld). Als voor gevoelige verificatiegegevens worden ontvangen, zorgen dat alle gegevens onherstelbare na voltooiing van het autorisatieproces. 
- Er is een zakelijke rechtvaardiging en 
- De gegevens worden veilig opgeslagen.
Voor gevoelige verificatiegegevens bevat de gegevens, zoals vermeld in de volgende vereisten 3.2.1 via 3.2.3:


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore niet verwijderen of vernietigen eventuele opgeslagen CHD; de voorbeeldgegevens wordt voor demonstratiedoeleinden alleen opgeslagen. Alle gegevens is echter versleuteld en geen primaire account nummer (PAN) gegevens worden opgeslagen.<br /><br />|



### <a name="pci-dss-requirement-321"></a>PCI-DSS vereiste 3.2.1

**3.2.1** niet de volledige inhoud van een nummer (van de magnetische stripe zich op de achterkant van een kaart, gelijkwaardige egevens in een chip of elders) opslaan na autorisatie. Deze gegevens heet ook volledige bijhouden, bijhouden, nummer 1, 2 en magnetische stripe gegevens bijhouden. 

> [!NOTE]
> In de normale gang van zaken moet de volgende gegevenselementen van de magnetische stripe mogelijk worden bewaard: 
> - De naam van de kaarthouder 
> - Primaire nummer (PAN) 
> - Vervaldatum 
> - Service-code 
>
> Om het risico minimaliseren, alleen deze gegevenselementen zo nodig voor bedrijven worden opgeslagen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore worden niet opgeslagen voor de volledige inhoud van een CHD.|



### <a name="pci-dss-requirement-322"></a>PCI-DSS vereiste 3.2.2

**3.2.2** Sla niet op de kaart verificatiecode of -waarde (drie cijfers of met vier cijfers getal afgedrukt op de voorgrond of achter een betaling kaart gebruikt om te controleren van de kaart niet presenteren transacties) na autorisatie.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore versleutelt alle gegevens, inclusief de CVW-voorbeelden.|



### <a name="pci-dss-requirement-323"></a>PCI-DSS vereiste 3.2.3

**3.2.3** Sla geen pincode (PIN) of het versleutelde PINCODE blok na autorisatie.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat geen PINCODE-informatie.|



## <a name="pci-dss-requirement-33"></a>PCI-DSS vereiste 3.3

**3.3** masker PANNEN wanneer weergegeven (de eerste zes en de laatste vier cijfers zijn het maximum aantal cijfers moet worden weergegeven), zodanig dat alleen medewerkers met een legitieme bedrijf moeten het volledige PAN. kunnen zien 

> [!NOTE]
> Deze vereiste is geen vervanging voor strengere vereisten voor het weergeven van gegevens van de kaarthouder — bijvoorbeeld, juridische of betaling doen kaart merkvereisten voor ontvangstbevestigingen voor verkooppunten (POS).

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maskeert het hoofdaccount nummer (PAN) met transparante gegevensversleuteling, altijd versleutelde kolommen en dynamische gegevensmaskering. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-34"></a>PCI-DSS vereiste 3.4

**3.4** onleesbaar PAN overal worden opgeslagen (inclusief op draagbare mediabestanden, back-upmedium, en in Logboeken) met behulp van de volgende methoden:
- One-Way hashes op basis van sterke cryptografie (hash moet van het gehele PANNEN)
- Moet worden afgekapt (hashing kan niet worden gebruikt ter vervanging van de afgekapte segment van PANNEN)
- Index tokens en dergelijke (pad moeten worden veilig opgeslagen)
- Sterke cryptografie met bijbehorende Sleutelbeheer processen en procedures. 

> [!NOTE]
> Er is een relatief trivial moeite voor een kwaadwillende persoon kan nieuw oorspronkelijke PAN-gegevens als ze toegang tot zowel de afgekapte versie als de hash van een PAN. hebben Indien hashing en afgekapt versies van dezelfde PAN aanwezig zijn in een entiteit omgeving, extra controles moeten worden voldaan om ervoor te zorgen dat de hash en afgekapte versies voor het oorspronkelijke PAN. Reconstrueer kunnen niet worden gecorreleerd

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore versleutelt alle gegevens die creditcard en maakt gebruik van Azure Key Vault voor het beheren van sleutels, het ophalen van CHD voorkomen.<br /><br />Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-341"></a>PCI-DSS vereiste 3.4.1

**3.4.1** als schijfversleuteling wordt gebruikt (in plaats van de versleuteling van de database bestandsniveau of kolom), logische toegang moet worden beheerd afzonderlijk en onafhankelijk van systeemeigen besturingssysteem authenticatie en access mechanismen (bijvoorbeeld door niet met behulp van lokale gebruiker accountdatabases of aanmeldingsreferenties algemeen netwerk). Er mag geen ontsleutelingssleutels gekoppeld aan gebruikersaccounts. 

> [!NOTE]
> Deze vereiste geldt naast de andere PCI DSS-codering en -sleutelbeheer vereisten.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore versleutelt alle opgeslagen gegevens en scheidt verkeer om te voorkomen dat bevoegde uitbreiding van bevoegdheden voor DevOps-functies.<br /><br />Als de App-serviceomgeving is beveiligd en vergrendeld, moet er een mechanisme om toe te staan voor DevOps releases of wijzigingen die mogelijk nodig, zoals de mogelijkheid voor het bewaken van een Web-App met behulp van Kudu.<br /><br />Een virtuele machine wordt ingesteld als een jumpbox (bastion host) met de volgende configuraties:<br /><br /><ul><li>[Antimalware-uitbreiding](/azure/security/azure-security-antimalware)</li><li>[Extensie OMS Monitoring](/azure/virtual-machines/virtual-machines-windows-extensions-oms)</li><li>[Extensie voor diagnostische gegevens VM](/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)</li><li>[BitLocker-versleutelde schijf](/azure/security/azure-security-disk-encryption)</li></ul>Met Azure Key Vault wordt uitgelijnd met Azure Government, PCI DSS en HIPAA-vereisten.|



## <a name="pci-dss-requirement-35"></a>PCI-DSS vereiste 3.5

**3.5** maken en implementeren van documenten procedures voor het beveiligen van sleutels die worden gebruikt voor het beveiligen van de kaarthouder opgeslagen gegevens tegen openbaarmaking en misbruik. 

> [!NOTE]
> Deze vereiste geldt voor sleutels die worden gebruikt om kaarthouder opgeslagen gegevens te versleutelen en geldt ook voor het versleutelen van de sleutel sleutels die worden gebruikt voor het beveiligen sleutels gegevens versleutelen: zulke sleutels voor het versleutelen van de sleutel moet ten minste even sterk als de sleutel voor het versleutelen van gegevens.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:**<br /><br />Microsoft Azure zorgt ervoor dat de klant sleutelkluizen zijn logisch van elkaar zijn afgezonderd en logisch is geïsoleerd van het beheervlak van de Sleutelkluis-service. Sleutelkluis is zodanig ontworpen dat Microsoft geen permanente toegang tot de sleutelkluis van de klant heeft. <br /><br />Sleutels worden beveiligd door Microsoft Azure industriestandaard algoritmen, sleutellengten en hardware security modules (HSM's).<br /><br />Een sleutel die wordt opgeslagen in Microsoft Azure Key Vault kan worden gebruikt om een andere sleutel te beveiligen. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore bevat documentatie die u laten zien en helpen bij het implementeren van een beveiligde sleutel oplossing ter bescherming van de demo CHD.|



### <a name="pci-dss-requirement-351"></a>PCI-DSS vereiste 3.5.1

**3.5.1** *aanvullende vereisten voor alleen-providers:* onderhouden van een gedocumenteerde beschrijving van de cryptografische architectuur die bevat:
- Details van alle algoritmen, protocollen en de sleutels die worden gebruikt voor de beveiliging van de kaarthouder-gegevens, inclusief belangrijke sterkte en de uiterste datum
- Beschrijving van het sleutelgebruik voor elke sleutel
- Inventaris van een HSM's en andere SCDs gebruikt voor sleutelbeheer 

> [!NOTE]
> Deze vereiste is een aanbevolen procedure tot en met 31 januari 2018, waarna een vereiste wordt.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:**<br /><br />Microsoft Azure zorgt ervoor dat de klant sleutelkluizen zijn logisch van elkaar zijn afgezonderd en logisch is geïsoleerd van het beheervlak van de Sleutelkluis-service. Sleutelkluis is zodanig ontworpen dat Microsoft geen permanente toegang tot de sleutelkluis van de klant heeft. <br /><br />Sleutels worden beveiligd door Microsoft Azure industriestandaard algoritmen, sleutellengten en hardware security modules (HSM's).<br /><br />Een sleutel die wordt opgeslagen in Microsoft Azure Key Vault kan worden gebruikt om een andere sleutel te beveiligen. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore bevat documentatie die u laten zien en helpen bij het implementeren van een beveiligde sleutel oplossing ter bescherming van de demo CHD.|



### <a name="pci-dss-requirement-352"></a>PCI-DSS vereiste 3.5.2

**3.5.2** toegang beperken tot de cryptografische sleutels aan het minste aantal bewaarders nodig.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:**<br /><br />Sleutelkluis ondersteunt gedetailleerde toegangsbeleid, waardoor de eigenaar van een Sleutelkluis toegang verlenen tot specifieke functionaliteit specifieke bewerkingen aan specifieke entiteiten uit te voeren. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Contoso Webstore Sleutelbeheer is geïsoleerd, zodat één gebruikersaccount (admin ##@contosowebstore.com).|



### <a name="pci-dss-requirement-353"></a>PCI-DSS vereiste 3.5.3

**3.5.3** geheime en persoonlijke sleutels die worden gebruikt voor het versleutelen/ontsleutelen van gegevens van de kaarthouder in (of meerdere) van de volgende formulieren te allen tijde opslaan:
- Versleuteld met een sleutel versleutelen van de sleutel die ten minste even sterk als de sleutel voor het versleutelen van gegevens, en die afzonderlijk van de sleutel voor het versleutelen van gegevens is opgeslagen
- In een beveiligde cryptografische apparaat (zoals een (host) hardware security module (HSM) of punten goedgekeurd punt van interactie apparaat)
- Geaccepteerd als ten minste twee volledige belangrijke onderdelen of sleutel shares, in overeenstemming met een industrie - methode 

> [!NOTE]
> Het is niet vereist dat de openbare sleutels worden opgeslagen in een van deze formulieren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:**<br /><br />Sleutels worden opgeslagen in de specifieke sleutelkluizen geïdentificeerd door de klant.<br /><br />Sleutelkluis toegankelijk tegelijk en globaal door meerdere toepassingen die vermindert de noodzaak voor het kopiëren van een sleutel en opslaan op meerdere locaties. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-354"></a>PCI-DSS vereiste 3.5.4

**3.5.4** cryptografische sleutels opslaan in het minste aantal mogelijke locaties.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:**<br /><br />Sleutels worden opgeslagen in de specifieke sleutelkluizen geïdentificeerd door de klant. <br /><br />Sleutelkluis toegankelijk tegelijk en globaal door meerdere toepassingen die vermindert de noodzaak voor het kopiëren van een sleutel en opslaan op meerdere locaties. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



## <a name="pci-dss-requirement-36"></a>PCI-DSS vereiste 3.6

**3.6** volledig documenteren en implementeren van alle Sleutelbeheer processen en procedures voor cryptografische sleutels die worden gebruikt voor het versleutelen van gegevens van de kaarthouder, waaronder de volgende. 

> [!NOTE]
> Talrijke industrienormen voor sleutelbeheer zijn beschikbaar in verschillende bronnen, met inbegrip van NIST, die kan worden gevonden op http://csrc.nist.gov.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-361"></a>PCI-DSS vereiste 3.6.1

**3.6.1** genereren van sleutels voor sterke cryptografie

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:** <br /><br />Bij het genereren van sleutels in de Sleutelkluis, is Azure verantwoordelijk voor het genereren van sleutels per specificaties van de klant. Sleutels worden gegenereerd met behulp van een HSM. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-362"></a>PCI-DSS vereiste 3.6.2

**3.6.2** distributie van cryptografische sleutels beveiligen

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:**<br /><br />Het bring uw eigen hulpprogramma key (BYOK) kapselt de sleutel van de klant en gericht is op een specifieke beveiligings-kluis die is gekoppeld aan een specifieke Azure-abonnement. De sleutel kan alleen worden geïmporteerd in het opgegeven abonnement sleutelkluis, in de opgegeven regio. Dit proces maakt gebruik van de procedures van de versleuteling geleverd door de hardwarefabrikant. Klanten ontvangen een melding dat de overdracht voltooid is. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-363"></a>PCI-DSS vereiste 3.6.3

**3.6.3** cryptografische sleutels, de opslag beveiligen

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:**<br /><br />Sleutels worden opgeslagen in de HSM's en zijn beveiligd met de cryptografische beveiliging van de fabrikant van de hardware. De metagegevens voor sleutels worden opgeslagen in Azure Storage in een versleutelde status die uniek is voor elke sleutelkluis. <br /><br /> |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-364"></a>PCI-DSS vereiste 3.6.4

**3.6.4** cryptografische sleutel wijzigingen voor sleutels die het einde van hun cryptoperiod bereikt (bijvoorbeeld, nadat een gedefinieerde periode is verstreken en/of nadat een bepaalde hoeveelheid gecodeerde tekst zijn geproduceerd door een opgegeven sleutel), zoals gedefinieerd door de gekoppelde de leverancier van toepassing of de eigenaar van de sleutel en op basis van andere aanbevolen procedures en richtlijnen (bijvoorbeeld NIST Special Publication 800-57).

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:**<br /><br />Sleutelkluis ondersteunt functionaliteit om te werken of draaien sleutels, die is gedefinieerd door de klant. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-365"></a>PCI-DSS vereiste 3.6.5

**3.6.5** buiten gebruik stellen of te vervangen (bijvoorbeeld, archiveren, vernietiging en/of intrekken) sleutels als noodzakelijk wanneer de integriteit van de sleutel is verzwakt (bijvoorbeeld vertrek van een werknemer met kennis van een sleutel versleutelde tekst onderdeel), of wordt vermoed dat sleutels veilig. 

> [!NOTE]
> Als het buiten gebruik gesteld of vervangen van de cryptografische sleutels moeten worden bewaard, moeten deze sleutels veilig worden gearchiveerd (bijvoorbeeld via een coderingssleutel voor de sleutel). Gearchiveerde cryptografische sleutels mag alleen worden gebruikt voor ontsleuteling/verificatiedoeleinden.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:**<br /><br />Sleutelkluis ondersteunt functionaliteit voor het buiten gebruik stelt of vervangen, die is gedefinieerd door de klant. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-366"></a>PCI-DSS vereiste 3.6.6

**3.6.6** als handmatige ongecodeerde cryptografische sleutel-management-bewerkingen worden gebruikt, deze bewerkingen moeten worden beheerd met gesplitste kennis en dubbele control. 

> [!NOTE]
> Voorbeelden van handmatige Sleutelbeheer bewerkingen bevatten, maar zijn niet beperkt tot: sleutel genereren, verzending, laden, opslag en vernietigd.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-367"></a>PCI-DSS vereiste 3.6.7

**3.6.7** voorkoming van onrechtmatige vervanging van de cryptografische sleutels.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | **Voor klanten met Sleutelkluis:**<br /><br />Sleutel kluizen logisch van elkaar worden gescheiden en bieden geen ondersteuning voor autorisatie van cross-directory. Als gevolg hiervan wordt niet-geautoriseerde vervanging voorkomen. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-368"></a>PCI-DSS vereiste 3.6.8

**3.6.8** vereiste voor cryptografische sleutel bewaarders formeel bevestigt dat ze begrijpt en accepteert u hun verantwoordelijkheden sleutel vallen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Contoso Webstore Sleutelbeheer is geïsoleerd, zodat één gebruikersaccount (admin ##@contosowebstore.com).|



## <a name="pci-dss-requirement-37"></a>PCI-DSS vereiste 3.7

**3.7** Zorg ervoor dat beveiligingsbeleid en operationele procedures voor het beveiligen van gegevens opgeslagen kaarthouder gedocumenteerd, dat in gebruik is, en bekend is dat alle betrokken partijen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Sleutelkluis voor alle Sleutelbeheer. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|





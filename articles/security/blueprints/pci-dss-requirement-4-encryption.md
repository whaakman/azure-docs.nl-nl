---
title: Azure betaling verwerking blauwdruk - versleutelingsvereisten
description: PCI-DSS vereiste 4
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 43f75ba9-cb4e-49ab-b3f4-09e48310bc18
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 3eb5b663558c2a68c13368b179ff942dd3c53716
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="encryption-requirements-for-pci-dss-compliant-environments"></a>Versleutelingsvereisten voor PCI DSS-compatibele omgevingen 
## <a name="pci-dss-requirement-4"></a>PCI-DSS vereiste 4

**Overdracht van gegevens van de kaarthouder coderen via open, op openbare netwerken**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Gevoelige gegevens moet worden versleuteld tijdens de verzending via netwerken die zijn gemakkelijk toegankelijk is voor kwaadwillende personen. Onjuist geconfigureerde draadloze netwerken voor en zwakke plekken in oudere codering en -verificatieprotocollen blijven doelen van kwaadwillende personen die misbruikt om bevoorrechte toegang krijgen tot kaarthouder gegevens omgevingen.

## <a name="pci-dss-requirement-41"></a>PCI-DSS vereiste 4.1

**4.1** sterke cryptografie en beveiliging protocollen (bijvoorbeeld TLS, IPSEC, SSH, enz.) ter bescherming van gevoelige kaarthouder gegevens tijdens de verzending via open, op openbare netwerken, waaronder de volgende gebruiken:
- Alleen vertrouwde sleutels en certificaten worden geaccepteerd.
- De gebruikte protocol ondersteunt alleen beveiligde versies of configuraties.
- De sterkte van de codering is geschikt voor de methode versleuteling gebruikt. 

> [!NOTE]
> Wanneer vroege SSL/TLS wordt gebruikt, kan de vereisten in de bijlage A2 moeten worden uitgevoerd.
>
> Voorbeelden van open, op openbare netwerken omvatten, maar zijn niet beperkt tot:
> - Het Internet
> - Draadloze technologieën, zoals 802.11 en Bluetooth
> - Mobiel technologieën, bijvoorbeeld Global System for Mobile communications (GSM) Code deling meerdere toegang (CDMA)
> - Algemene pakket Radio Service (GPRS)
> - Satellietcommunicatie


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is een PaaS-oplossing waarmee sterke cryptografie voor de implementatie als volgt:<br /><br />Om te voldoen aan de vereisten van versleutelde gegevens in rust, [Azure Storage](https://azure.microsoft.com/services/storage/) gebruikgemaakt van de volgende:<br /><br /><ul><li>[Azure Storage-versleuteling (SSE) voor gegevens in rust](/azure/storage/storage-service-encryption)</li><li>SQL Database: Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).</li><li>[Azure Disk Encryption (Bitlocker)](/azure/security/azure-security-disk-encryption)</li></ul>Met Azure Key Vault wordt uitgelijnd met Azure Government, PCI DSS en HIPAA-vereisten.|



### <a name="pci-dss-requirement-411"></a>PCI-DSS vereiste 4.1.1

**4.1.1** draadloze netwerken overbrengen van gegevens van de kaarthouder zorg, of verbonden met de gegevens kaarthouder omgeving, beste praktijken (bijvoorbeeld IEEE 802.11i) gebruiken voor het implementeren van sterke codering voor verificatie en verzending.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | SNMP- en draadloze netwerken worden niet geïmplementeerd in de oplossing.|



## <a name="pci-dss-requirement-42"></a>PCI-DSS vereiste 4.2

**4.2** onbeveiligde pannen nooit verzenden door eindgebruikers messaging technologieën (bijvoorbeeld e-mail, instant messaging, SMS, chat, enz.).

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore heeft geen elke messaging oplossing wordt geïmplementeerd en die niet-beveiligde primaire account nummer (PAN) gegevens worden verzonden.|



## <a name="pci-dss-requirement-43"></a>PCI-DSS vereiste 4.3

**4.3** Zorg ervoor dat beveiligingsbeleid en operationele procedures voor het versleutelen van gegevens van de kaarthouder van gegevensoverdrachten gedocumenteerd, dat in gebruik is, en bekend is dat alle betrokken partijen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het vastleggen en coderen gegevensoverdrachten met gegevens van de kaarthouder.|





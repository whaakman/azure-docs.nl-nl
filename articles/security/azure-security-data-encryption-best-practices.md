---
title: Aanbevolen procedures voor beveiliging van gegevens en versleuteling | Microsoft Docs
description: In dit artikel biedt een set van aanbevolen procedures voor beveiliging van gegevens en ingebouwde mogelijkheden van Azure met behulp van versleuteling.
services: security
documentationcenter: na
author: barclayn
manager: mbalwin
editor: TomSh
ms.assetid: 17ba67ad-e5cd-4a8f-b435-5218df753ca4
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/19/2018
ms.author: barclayn
ms.openlocfilehash: 686d4a8ac5239af12206b57072cc00aa10114d79
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976508"
---
# <a name="azure-data-security-and-encryption-best-practices"></a>Aanbevolen procedures voor Azure-gegevensbeveiliging en -versleuteling

Ter bescherming van gegevens in de cloud, moet u rekening voor de mogelijke statussen in die uw gegevens zich kunnen voordoen en welke besturingselementen beschikbaar zijn voor die status zijn. Aanbevolen procedures voor Azure-gegevensbeveiliging en -versleuteling betrekking op de volgende gegevens statussen:

- AT-rest: Dit omvat alle informatie opslagobjecten, containers, en typen die statisch bestaat op de fysieke media of magnetische of optische schijven.
- Tijdens de overdracht: Wanneer gegevens worden overgebracht tussen onderdelen, locaties of programma's, is het in-transit. Voorbeelden zijn overdracht via het netwerk via een servicebus (van on-premises naar cloud en vice versa, met inbegrip van hybride verbindingen, zoals ExpressRoute), of tijdens een i/o-proces.

In dit artikel bespreken we een aantal Azure-gegevens en encryption best practices. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure-gegevensbeveiliging en -versleuteling en de ervaringen van klanten zoals zelf.

Voor elke best practice wordt uitgelegd:

* Wat de beste manier is
* Waarom u deze wilt inschakelen, die best practices
* Wat kan het resultaat zijn als u een failover naar de aanbevolen procedure inschakelen
* Mogelijke alternatieven voor de aanbevolen procedure
* Hoe u meer informatie kunt krijgen om in te schakelen van de aanbevolen procedure

In dit artikel Azure-gegevensbeveiliging en aanbevolen procedures voor versleuteling is gebaseerd op een advies consensus en mogelijkheden van Azure-platform en functiesets, zoals ze bestaan op het moment dat dit artikel is geschreven. Adviezen en -technologieën na verloop van tijd worden gewijzigd en regelmatig op basis van deze wijzigingen in dit artikel wordt bijgewerkt.

## <a name="choose-a-key-management-solution"></a>Kies een oplossing voor sleutelbeheer

Beveiligen van uw sleutels is essentieel voor het beveiligen van uw gegevens in de cloud.

[Azure Key Vault](../key-vault/key-vault-overview.md) beveiligt cryptografische sleutels en geheimen die toepassingen en services in de cloud gebruiken. Key Vault stroomlijnt het beheerproces voor sleutels en zorgt dat u de controle houdt over de sleutels waarmee uw gegevens toegankelijk zijn en worden versleuteld. Ontwikkelaars kunnen sleutels voor ontwikkelen en testen in minuten maken en ze vervolgens te migreren naar productiesleutels die worden. Beveiligingsadministrator kunnen wanneer dit nodig is machtigingen aan sleutels verlenen (en intrekken).

U kunt Key Vault gebruiken voor het maken van meerdere beveiligde containers, die kluizen worden genoemd. Deze kluizen worden ondersteund door HSM's. Kluizen verminderen de kans op onbedoeld verlies van beveiligingsinformatie door de opslag van toepassingsgeheimen te centraliseren. Sleutelkluizen bepaalt ook en meld u de toegang tot alles wat erin is opgeslagen. Azure Key Vault kan verwerken aanvragen en vernieuwen van Transport Layer Security (TLS)-certificaten. Het biedt functies voor een robuuste oplossing voor de levensduur van certificaten beheren.

Azure Key Vault is ontworpen ter ondersteuning van toepassingssleutels en -geheimen. Key Vault is niet bedoeld om te worden van een archief voor wachtwoorden van gebruikers.

Hieronder vindt u aanbevolen beveiligingsprocedures voor het gebruik van Key Vault.

**Beste**: Toegang verlenen aan gebruikers, groepen en toepassingen binnen een bepaald bereik.   
**Details**: Gebruik vooraf gedefinieerde van RBAC-rollen. Bijvoorbeeld, om toegang te verlenen aan een gebruiker voor het beheren van sleutelkluizen, zou u de vooraf gedefinieerde rol toewijzen [Inzender voor Key Vault](../role-based-access-control/built-in-roles.md) aan deze gebruiker op een bepaald bereik. Het bereik zou in dit geval zijn een abonnement, een resourcegroep of alleen een specifieke key vault. Als de vooraf gedefinieerde rollen niet aansluiten bij uw behoeften, kunt u [uw eigen rollen definiëren](../role-based-access-control/custom-roles.md).

**Beste**: Bepalen welke gebruikers toegang hebben tot.   
**Details**: Toegang tot een Key Vault wordt geregeld via twee afzonderlijke interfaces: de beheerlaag en de gegevenslaag. Het toegangsbeheer van de beheerlaag en het toegangsbeheer van de gegevenslaag werken onafhankelijk van elkaar.

Gebruik RBAC om te bepalen welke gebruikers toegang hebben tot. Als u toegang verlenen een toepassing wilt voor het gebruik van sleutels in een key vault, hoeft u alleen te verlenen van machtigingen voor toegang tot de gegevenslaag door toegangsbeleid van key vault gebruiken en geen toegang tot de beheerlaag is nodig voor deze toepassing. Daarentegen als u wilt dat een gebruiker kan eigenschappen van de kluis lezen en tags maar geen toegang heeft tot de sleutels, geheimen of certificaten, kunt u deze gebruiker lezen-toegang verlenen met behulp van RBAC en geen toegang tot het gegevensvlak is vereist.

**Beste**: Store certificaten in uw key vault. De certificaten zijn van hoge waarde. In de verkeerde handen vallen kan de beveiliging van uw toepassing of de beveiliging van uw gegevens worden aangetast.   
**Details**: Azure Resource Manager kunt veilig implementeren van certificaten die zijn opgeslagen in Azure Key Vault naar Azure VM's wanneer de virtuele machines worden geïmplementeerd. Door in te stellen geschikte toegangsbeleid voor key vault, bepalen u ook wie er toegang tot uw certificaat. Een ander voordeel is dat u al uw certificaten op één locatie in Azure Key Vault beheren. Zie [certificaten implementeren op virtuele machines uit de Sleutelkluis door de klant beheerde](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) voor meer informatie.

**Beste**: Zorg ervoor dat u de verwijdering van sleutelkluizen of key vault-objecten kunt herstellen.   
**Details**: Verwijdering van sleutel-kluizen of key vault-objecten kunnen onbedoelde of schadelijke. Schakel de functie voor voorlopig verwijderen en verwijderen van functies voor gegevensbeveiliging van Key Vault, met name voor sleutels die worden gebruikt om gegevens in rust te versleutelen. Verwijderen van deze sleutels is gelijk aan het verlies van gegevens, zodat u kunt herstellen van verwijderde kluizen en objecten kluis, indien nodig. Oefen Key Vault-recovery-bewerkingen op gezette tijden.

> [!NOTE]
> Als een gebruiker inzendersrechten (RBAC) voor de beheerlaag van een sleutelkluis heeft, kunnen ze zichzelf toegang verlenen tot de gegevenslaag door in te stellen van een toegangsbeleid voor key vault. Het is raadzaam dat u precies wie heeft Inzender-toegang tot uw key vaults bepalen, om ervoor te zorgen dat alleen gemachtigde personen kunnen openen en beheren van uw sleutelkluizen, sleutels, geheimen en certificaten.
>
>

## <a name="manage-with-secure-workstations"></a>Beheren met beveiligde werkstations

> [!NOTE]
> De beheerder van abonnement of de eigenaar moet een werkstation voor beveiligde toegang of een werkstation met bevoegde toegang te gebruiken.
>
>

Omdat de meeste aanvallen zijn gericht de eindgebruiker, wordt het eindpunt een van de primaire punten van een aanval. Een aanvaller die het eindpunt wordt aangetast, kunt de referenties van de gebruiker toegang te krijgen tot gegevens van de organisatie gebruiken. De meeste endpoint-aanvallen te profiteren van het feit dat gebruikers beheerders in hun lokale werkstation zijn.

**Beste**: Gebruik een beveiligd beheerwerkstation gevoelige accounts, taken en gegevens te beschermen.   
**Details**: Gebruik een [werkstation met bevoegde toegang](https://technet.microsoft.com/library/mt634654.aspx) te verminderen van de kwetsbaarheid van werkstations. Deze beveiligde beheerwerkstations kunt u sommige van deze aanvallen te beperken en zorg ervoor dat uw gegevens veiliger.

**Beste**: Zorg ervoor dat endpoint protection.   
**Details**: Beveiligingsbeleid afdwingen op alle apparaten die worden gebruikt voor het gebruiken van gegevens, ongeacht de locatie van gegevens (cloud of on-premises).

## <a name="protect-data-at-rest"></a>Gegevens in rust beveiligen

[Gegevensversleuteling in rust](https://cloudblogs.microsoft.com/microsoftsecure/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) is een verplichte stap voor privacy, naleving en onafhankelijkheid van gegevens.

**Beste**: Schijfversleuteling voor het beveiligen van uw gegevens toepassen.   
**Details**: Gebruik [Azure Disk Encryption](azure-security-disk-encryption.md). Hiermee kunt IT-beheerders voor het versleutelen van Windows en Linux IaaS-VM-schijven. Schijfversleuteling combineert de industriestandaard Windows BitLocker-functie en de dm-crypt-functie van Linux om volumeversleuteling voor het besturingssysteem en de gegevensschijven te bieden.

Azure Storage en Azure SQL Database versleutelen van data-at-rest standaard, en veel services bieden versleuteling als een optie. Azure Key Vault kunt u sleutels die u kunt openen en versleutelen van uw gegevens beheren. Zie [ondersteuning voor Azure-resource providers codering model voor meer informatie](azure-security-encryption-atrest.md#azure-resource-providers-encryption-model-support).

**Aanbevolen procedures**: Versleuteling gebruiken om te beperken van risico's met betrekking tot onbevoegde toegang tot gegevens.
**Details**: Uw schijven versleutelen voordat u gevoelige gegevens naar die stations wegschrijven.

Organisaties die geen gegevensversleuteling afdwingen worden meer blootgesteld aan problemen met de vertrouwelijkheid van gegevens. Onbevoegde of rogue gebruikers kunnen bijvoorbeeld gegevens in de accounts waarmee is geknoeid stelen of ongeoorloofde toegang verlenen tot gegevens gecodeerd in duidelijke indeling. Bedrijven moeten ook bewijzen dat ze toegewijd en met juiste beveiligingsmaatregelen zijn voor het verbeteren van de beveiliging van hun gegevens om te voldoen aan regelgeving vanuit de sector.

## <a name="protect-data-in-transit"></a>Gegevens tijdens overdracht beveiligen

Beveiligen van gegevens die onderweg zijn, moet een essentieel onderdeel van uw strategie voor gegevensbescherming. Omdat gegevens heen en van veel locaties verplaatsen is, het algemeen wordt aangeraden dat u altijd SSL/TLS-protocollen gebruikt voor het uitwisselen van gegevens op verschillende locaties. In sommige gevallen wilt u mogelijk het hele communicatiekanaal tussen uw on-premises en cloud isoleren infrastructuren met behulp van een VPN.

Voor gegevens verplaatsen tussen uw on-premises infrastructuur en Azure, kunt u passende beveiligingsmaatregelen zoals HTTPS- of VPN. Gebruiken bij het verzenden van versleuteld verkeer tussen een Azure-netwerk en een on-premises locatie via het openbare internet [Azure VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/).

Hieronder vindt u aanbevolen procedures die specifiek zijn voor het gebruik van Azure VPN-Gateway, SSL/TLS- en HTTPS.

**Beste**: Veilige toegang vanaf meerdere werkstations zich on-premises naar een Azure-netwerk.   
**Details**: Gebruik [site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).

**Beste**: Veilige toegang vanaf een afzonderlijk werkstation zich on-premises naar een Azure-netwerk.   
**Details**: Gebruik [punt-naar-site VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

**Beste**: Verplaats grotere gegevenssets via een speciale snelle WAN-verbinding.   
**Details**: Gebruik [ExpressRoute](../expressroute/expressroute-introduction.md). Als u gebruiken van ExpressRoute wilt, kunt u ook de gegevens op het toepassingsniveau van de versleutelen met behulp van [SSL/TLS](https://support.microsoft.com/kb/257591) of andere protocollen voor extra beveiliging.

**Beste**: Werken met Azure Storage via Azure portal.   
**Details**: Alle transacties plaatsvinden via HTTPS. U kunt ook [REST API voor Storage](https://msdn.microsoft.com/library/azure/dd179355.aspx) via HTTPS om te communiceren met [Azure Storage](https://azure.microsoft.com/services/storage/) en [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

Organisaties die niet voldoen aan het beveiligen van gegevens die onderweg zijn, zijn vatbaar voor [man-in-the-middle-aanvallen](https://technet.microsoft.com/library/gg195821.aspx), [niet kan worden afgeluisterd](https://technet.microsoft.com/library/gg195641.aspx), en sessiehijacking. Deze aanvallen mag de eerste stap bij het toegang krijgen tot vertrouwelijke gegevens.

## <a name="secure-email-documents-and-sensitive-data"></a>Beveiligen van e-mail, documenten en gevoelige gegevens

U wilt beheren en beveiligen van e-mail, documenten en gevoelige gegevens die u buiten uw bedrijf delen. [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/) is een cloud-gebaseerde oplossing waarmee een organisatie te classificeren, labelen en beveiligen van documenten en e-mailberichten. Dit kan automatisch worden gedaan door beheerders die regels en voorwaarden door gebruikers of een combinatie waarbij gebruikers aanbevelingen krijgen handmatig te definiëren.

Classificatie kan worden geïdentificeerd op altijd, ongeacht waar de gegevens worden opgeslagen of met wie ze worden gedeeld. De labels bevatten visuele markeringen, zoals een koptekst, voettekst of watermerk. Metagegevens wordt toegevoegd aan bestanden en e-mailkopteksten in niet-versleutelde tekst. De ongecodeerde tekst zorgt ervoor dat andere services, zoals oplossingen voor preventie van gegevensverlies, kunnen de classificatie identificeren en passende actie kunnen ondernemen.

De beveiligingstechnologie maakt gebruik van Azure Rights Management (Azure RMS). Deze technologie is geïntegreerd met andere Microsoft-cloudservices en -toepassingen, zoals Office 365 en Azure Active Directory. Deze beveiligingstechnologie maakt gebruik van autorisatiebeleidsregels voor versleuteling, identiteit en. Blijft beveiliging die wordt toegepast via Azure RMS met de documenten en e‑mailberichten, onafhankelijk van de locatie: binnen of buiten uw organisatie, netwerken, bestandsservers en toepassingen.

Deze oplossing voor gegevensbeveiliging behoudt u de controle over uw gegevens, zelfs wanneer deze worden gedeeld met anderen. U kunt ook Azure RMS gebruiken met uw eigen line-of-business-toepassingen en gegevensbeveiligingsoplossingen van softwareleveranciers, ongeacht of deze toepassingen en oplossingen zich on-premises of in de cloud.

We raden aan dat u:

- [Azure Information Protection implementeert](https://docs.microsoft.com/azure/information-protection/deployment-roadmap) voor uw organisatie.
- Labels die overeenkomen met uw zakelijke vereisten van toepassing. Bijvoorbeeld: Een label met de naam 'zeer vertrouwelijk' voor alle documenten en e-mailberichten die topgeheime gegevens bevatten, om te classificeren en beveiligen van deze gegevens toepassen. Alleen geautoriseerde gebruikers kunnen vervolgens toegang tot deze gegevens, met de beperkingen die u opgeeft.
- Configureer [Gebruiksregistratie voor Azure RMS](https://docs.microsoft.com/azure/information-protection/log-analyze-usage) zodat u kunt controleren hoe uw organisatie gebruikmaakt van de beveiligingsservice.

Organisaties die zwakke op [gegevensclassificatie](https://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) en bestandsbeveiliging mogelijk gevoeliger voor gegevenslekken of misbruik van gegevens. Met de juiste Bestandsbeveiliging, kunt u gegevensstromen krijgt u inzicht in uw bedrijf, riskant gedrag detecteren en corrigerende maatregelen treffen, toegang tot documenten volgen, enzovoort analyseren.

## <a name="next-steps"></a>Volgende stappen

Zie [Azure-beveiliging aanbevolen procedures en patronen](security-best-practices-and-patterns.md) voor meer best practices voor beveiliging moeten worden gebruikt wanneer bent u het ontwerpen, implementeren en beheren van uw cloudoplossingen met behulp van Azure.

De volgende resources zijn beschikbaar, voor meer algemene informatie over Azure-beveiliging en gerelateerde Microsoft-services:
* [Azure-Team Beveiligingsblog](https://blogs.msdn.microsoft.com/azuresecurity/) - voor up-to-date informatie over het laatste nieuws over Azure-beveiliging
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx) - hier Microsoft beveiligingsproblemen, met inbegrip van problemen met Azure, kunnen u ook via e-mail secure@microsoft.com

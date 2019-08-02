---
title: Back-ups en herstel na nood gevallen in azure Australië
description: Back-ups en herstel na nood gevallen in Microsoft Azure voor Australische overheids instanties in relatie tot de ASD essentiële 8
author: galey801
ms.service: azure-australia
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 078918cbfeffb1ff5f3626b5add96bc6622a90a7
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68571521"
---
# <a name="backup-and-disaster-recovery-in-azure-australia"></a>Back-ups en herstel na nood gevallen in azure Australië

Het maken van back-ups en nood herstel plannen met de ondersteunende infra structuur is essentieel voor alle organisaties. Het belang van het hebben van een back-upoplossing is gemarkeerd door de insluiting in de [essentiële 8 van de Australische Cyber Security Center](https://acsc.gov.au/publications/protect/essential-eight-explained.htm).

Microsoft Azure biedt twee services die tolerantie mogelijk maken: Azure Backup en Azure Site Recovery. Met deze services kunt u uw gegevens, zowel on-premises als in de Cloud, beveiligen voor diverse ontwerp scenario's. Azure Backup en Azure Site Recovery gebruiken beide een algemene opslag-en beheer Bron: de Azure Recovery Services-kluis. Deze kluis wordt gebruikt om Azure Backup en Azure Site Recovery gegevens te beheren, te bewaken en te scheiden.

In dit artikel vindt u meer informatie over de belangrijkste ontwerp elementen voor het implementeren van Azure Backup en Azure Site Recovery in overeenstemming met de [informatie over de beveiligings handleidingen van het Australische signalen (ASD)-besturings elementen](https://acsc.gov.au/infosec/ism/index.htm).

## <a name="azure-backup"></a>Azure Backup

![Azure Backup](media/backup-overview.png)

Azure Backup lijkt op een traditionele on-premises back-upoplossing en biedt de mogelijkheid om back-ups te maken van zowel on-premises als door Azure gehoste gegevens. Azure Backup kan worden gebruikt om een back-up te maken van de volgende gegevens typen naar Azure:

* Bestanden en mappen
* Ondersteunde Windows-en Linux-besturings systemen die worden gehost op:
  * Hyper-V-en VMWare-Hyper Visor
  * Fysieke hardware
* Ondersteunde micro soft-toepassingen

### <a name="azure-site-recovery"></a>Azure Site Recovery

![Azure Site Recovery](media/asr-overview.png)

Azure Site Recovery worden werk belastingen gerepliceerd die bestaan uit één virtuele machine of toepassingen met meerdere lagen. Replicatie wordt van on-premises naar Azure ondersteund, tussen Azure-regio's of tussen on-premises locaties die door Azure Site Recovery worden georganiseerd. On-premises virtuele machines kunnen worden gerepliceerd naar Azure of naar een ondersteunde on-premises Hyper Visor. Zodra de configuratie is geconfigureerd, organiseert Azure Site Recovery de replicatie, failover en failback.

## <a name="key-design-considerations"></a>Belangrijkste overwegingen bij het ontwerpen

Bij het implementeren van een oplossing voor back-ups of nood herstel moet u rekening houden met de voorgestelde oplossing:

* Het bereik en het volume van de gegevens die moeten worden vastgelegd
* Hoe lang de gegevens behouden blijven
* Hoe deze gegevens veilig worden opgeslagen en beheerd
* De geografische locaties waar de gegevens worden opgeslagen
* Routine matige systeem test procedures

De ISM biedt richt lijnen voor de beveiligings overwegingen die moeten worden gemaakt bij het ontwerpen van een oplossing. Microsoft Azure biedt u de mogelijkheid om deze beveiligings overwegingen te verhelpen.

### <a name="data-sovereignty"></a>Data soevereiniteit

Organisaties moeten ervoor zorgen dat data soevereiniteit wordt gehandhaafd bij het gebruik van opslag locaties op basis van de Cloud. Azure Policy biedt de mogelijkheid om de toegestane locaties te beperken waar een Azure-resource kan worden gemaakt. De ingebouwde Azure Policy ' toegestane locaties ' wordt gebruikt om ervoor te zorgen dat alle Azure-resources die zijn gemaakt onder het bereik van een toegewezen Azure Policy alleen kunnen worden gemaakt op de aangewezen geografische locaties.

De Azure Policy items voor geografische beperking voor Azure-resources zijn:

* allowedLocations
* allowedSingleLocation

Met deze beleids regels kunnen Azure-beheerders het maken beperken tot een lijst van locaties die zijn aangewezen of zelfs als één geografische locatie.

### <a name="redundant-and-geographically-dispersed-storage"></a>Redundante en geografisch verspreide opslag

Gegevens die zijn opgeslagen in de Azure Recovery service-kluis, worden altijd opgeslagen op redundante opslag. De Recovery service-kluis maakt standaard gebruik van Azure geografisch redundante opslag (GRS). Gegevens die zijn opgeslagen met behulp van GRS, worden gerepliceerd naar andere Azure-data centers in de [secundaire regio](https://docs.microsoft.com/azure/best-practices-availability-paired-regions)van de Recovery service-kluis. Deze gerepliceerde gegevens worden opgeslagen als alleen-lezen en worden alleen beschrijfbaar gemaakt als er een Azure failover-gebeurtenis is. Binnen het Azure-Data Center worden de gegevens gerepliceerd tussen afzonderlijke fout domeinen en upgrade domeinen om de kans op hardware of op onderhoud gebaseerde storingen te minimaliseren. GRS biedt jaarlijks ten minste 99.99999999999999% Beschik baarheid.

De Azure Recovery Services-kluis kan worden geconfigureerd voor gebruik van lokaal redundante opslag (LRS). LRS is een voordelige opslag optie met de afweging van de lagere Beschik baarheid. Dit model voor redundantie maakt gebruik van dezelfde replicatie tussen afzonderlijke fout domeinen en upgrade domeinen, maar wordt niet gerepliceerd tussen geografische regio's. Gegevens die zich in LRS-opslag bevinden, maar niet zo veer krachtig als GRS, hebben jaarlijks nog mini maal 99,999999999% Beschik baarheid.

In tegens telling tot traditionele externe opslag technologieën zoals tape media, worden de extra kopieën van de gegevens automatisch gemaakt en is er geen extra administratieve overhead vereist.

### <a name="restricted-access-and-activity-monitoring"></a>Beperkte toegang en activiteiten bewaken

Back-upgegevens moeten worden beveiligd tegen beschadiging, wijziging en niet-goedgekeurde verwijdering. Zowel Azure Backup als Azure Site Recovery gebruik maken van de algemene Azure-beheer infrastructuur. Deze infra structuur biedt gedetailleerde controle, logboek registratie en op rollen gebaseerde Access Control (RBAC) naar bronnen die zich in azure bevinden. Toegang tot back-upgegevens kan worden beperkt tot het selecteren van administratieve mede werkers en alle acties waarbij back-upgegevens worden betrokken, kunnen worden vastgelegd en gecontroleerd.

Zowel Azure Backup als Azure Site Recovery hebben ingebouwde registratie-en rapportage functies. Problemen die zich voordoen tijdens een back-up of replicatie, worden gerapporteerd aan beheerders die gebruikmaken van de Azure-beheer infrastructuur.

Azure Recovery Services-kluis heeft ook de volgende aanvullende beveiligings maatregelen voor gegevens:

* Back-upgegevens worden 14 dagen na het verwijderen van een bewerking bewaard
* Waarschuwingen en meldingen voor kritieke bewerkingen, zoals ' back-up met Verwijder gegevens stoppen '
* Beveiligings pincode vereisten voor kritieke bewerkingen
* Er zijn mini maal controles op de Bewaar termijn aanwezig

Deze minimale Bewaar periode controles zijn onder andere:

* Voor dagelijks bewaren, mini maal zeven dagen retentie
* Voor een wekelijkse Bewaar periode van Mini maal vier weken
* Voor een maandelijkse Bewaar periode van Mini maal drie maanden
* Jaarlijks bewaren, mini maal één jaar van bewaren

Alle back-upgegevens die zijn opgeslagen in azure, worden versleuteld met behulp van de Storage Service Encryption (SSE) van Azure. Dit is standaard ingeschakeld voor alle nieuwe en bestaande opslag accounts en kan niet worden uitgeschakeld. De versleutelde gegevens worden automatisch ontsleuteld tijdens het ophalen. Gegevens die zijn versleuteld met SSE, worden standaard versleuteld met een sleutel die door micro soft wordt verschaft en beheerd. Organisaties kunnen ervoor kiezen om hun eigen versleutelings sleutel te bieden en te beheren voor gebruik met SSE. Dit biedt een optionele extra beveiligingslaag voor de versleutelde gegevens. Deze sleutel kan door de klant on-premises of veilig worden opgeslagen in de Azure-sleutel kluis.

### <a name="secure-data-transport"></a>Beveiligd gegevens transport

Azure Backup gegevens zijn versleuteld tijdens de overdracht met behulp van AES 256. Deze gegevens worden beveiligd via het gebruik van een wachtwoordzin die wordt gemaakt door administratief personeel wanneer de back-up voor het eerst wordt geconfigureerd. Micro soft heeft geen toegang tot deze wachtwoordzin, wat betekent dat de klant ervoor moet zorgen dat deze wachtwoordzin veilig wordt opgeslagen. De gegevens overdracht vindt vervolgens plaats tussen de on-premises omgeving en de Azure Recovery Services kluis via een beveiligde HTTPS-verbinding.  De gegevens in de Recovery Services kluis worden vervolgens op rest versleuteld met Azure SSE.

Azure Site Recovery gegevens worden ook in transit altijd versleuteld. Alle gerepliceerde gegevens worden veilig naar Azure getransporteerd met behulp van HTTPS en TLS. Wanneer een Azure-klant verbinding maakt met Azure met behulp van een ExpressRoute-verbinding, worden Azure Site Recovery gegevens verzonden via deze particuliere verbinding.  Wanneer een Azure-klant verbinding maakt met Azure via een VPN-verbinding, worden de gegevens via internet gerepliceerd tussen on-premises en de Recovery Services kluis.

Deze beveiligde netwerk gegevens overdracht verwijdert de beveiligings Risico's en beperkende vereisten voor het beheer van traditionele externe back-upopslag oplossingen zoals tape media.

### <a name="data-retention-periods"></a>Bewaar periode voor gegevens

Een minimale Bewaar periode van een back-up van drie maanden wordt aanbevolen, maar langere Bewaar perioden zijn meestal vereist. Azure Backup kunt Maxi maal 9999 kopieën van een back-up opgeven. Als er dagelijks een enkele Azure Backup van een beveiligd exemplaar is gemaakt, is het mogelijk dat de back-ups van 27 jaar dagelijks worden bewaard. Afzonderlijke maandelijkse back-ups van een beveiligde instantie staan 833 jaar aan retentie toe. Wanneer back-upgegevens verouderd zijn en er minder gedetailleerde back-ups worden gemaakt na verloop van tijd, wordt het totale Bewaar venster voor back-upgegevens verg root.  Azure beperkt de duur van de tijd dat gegevens in een Azure Recovery Services kluis blijven, maar alleen het totale aantal back-ups per exemplaar. Er is ook geen prestatie verschil tussen het herstellen van oude of nieuwe back-ups, waarbij elke herstel bewerking dezelfde hoeveelheid tijd in beslag neemt.

De Azure Recovery Services-kluis heeft een aantal standaard beleids regels voor back-ups en bewaren.  Beheer personeel kan ook een aangepast back-up-en bewaar beleid maken.

![Azure Backup beleid](media/create-policy.png)

Er moet een evenwicht tussen de back-upfrequentie en vereisten voor lange termijn retentie worden gevonden bij het configureren van Azure Backup-en bewaar beleid.

### <a name="backup-and-restore-testing"></a>Testen van back-ups en herstel

De ISM raadt aan om de back-upgegevens te testen om ervoor te zorgen dat de beveiligde gegevens geldig zijn wanneer een herstel bewerking of failover is vereist. Azure Backup en Azure Site Recovery bieden ook de mogelijkheid om beveiligde gegevens te testen zodra waarvan een back-up is gemaakt of is gerepliceerd. Gegevens die worden beheerd door Azure Backup kunnen worden hersteld naar een locatie waarnaar wordt geleid en de consistentie van de gegevens kan vervolgens worden gevalideerd.

Azure Site Recovery heeft ingebouwde mogelijkheden voor het uitvoeren van failover testen. Werk belastingen die zijn gerepliceerd naar de Recovery Services kluis, kunnen worden hersteld naar een aangewezen Azure-omgeving. De doel herstel omgeving kan volledig worden geïsoleerd van een productie omgeving om ervoor te zorgen dat er geen gevolgen zijn voor productie systemen tijdens het uitvoeren van een test. Zodra de test is voltooid, kunnen de test omgeving en alle resources direct worden verwijderd om de operationele kosten te verminderen.

Failover testen en validatie kunnen worden geautomatiseerd met behulp van de Azure Automation-service die in het Azure-platform is ingebouwd. Hierdoor kunnen failover-tests automatisch worden uitgevoerd om ervoor te zorgen dat gegevens naar Azure worden gerepliceerd.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg het artikel over het [beveiligen van Azure Policy](azure-policy.md).

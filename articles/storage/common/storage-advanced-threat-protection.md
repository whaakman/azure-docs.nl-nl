---
title: Bewaking van bedreigingen in Azure Storage
description: Azure Storage Advanced Threat Protection voor de detectie van afwijkingen in Accountactiviteit en een melding van mogelijk schadelijke pogingen om toegang te krijgen tot uw account configureren.
services: storage
author: rmatchoro
ms.service: storage
ms.topic: article
ms.date: 09/24/2018
ms.author: ronmat
ms.manager: shaik
ms.openlocfilehash: 8b2ca2d5d6418d68cab847df80fc437e468249ed
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46995636"
---
# <a name="azure-storage-advanced-threat-protection"></a>Azure Storage Advanced Threat Protection

Azure Storage Advanced Threat Protection afwijkingen in accountactiviteit detecteert en waarschuwt u van mogelijk schadelijke pogingen om toegang te krijgen tot uw account. Deze extra beveiliging kunt u om bedreigingen zonder de noodzaak om te worden van een beveiligingsexpert of beheren van controle beveiligingssystemen.

Bedreigingen worden opgehaald door de beveiligingswaarschuwingen die wordt geactiveerd wanneer het optreden van afwijkingen in de activiteit te definiëren. Deze waarschuwingen integreren met [Azure Security Center](https://azure.microsoft.com/services/security-center/) welke details van verdachte activiteiten en aanbevelingen voor het onderzoeken en bedreigingen verhelpen die op te nemen. 

> [!NOTE]
> Azure Storage Advanced Threat Protection is momenteel alleen beschikbaar voor de Blob-service. Beveiligingswaarschuwingen zijn geïntegreerd met Azure Security Center en via e-mail worden verzonden naar alle abonnementsbeheerders.

Azure Storage Advanced Threat Protection neemt diagnostische logboeken van lezen, schrijven en verwijderen van aanvragen voor Blob-service voor detectie van bedreigingen. Voor het onderzoeken van de waarschuwingen van Advanced Threat Protection, moet u [logboeken met diagnostische gegevens configureren](storage-monitor-storage-account.md#configure-logging) zodat alle niveaus van Logboeken voor de Blob-service.

## <a name="set-up-advanced-threat-protection-in-the-portal"></a>Advanced Threat Protection instellen in de portal

1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com/).

2. Navigeer naar de configuratiepagina van de Azure Storage-account dat u wilt beveiligen. In de **instellingen** weergeeft, schakelt **Advanced Threat Protection**.

3. In de **Advanced Threat Protection** configuratieblade
    * Schakel **ON** geavanceerde *Threat Protection*
    * Klik op **opslaan** de nieuwe of bijgewerkte Advanced Threat Protection-beleid op te slaan.

![Azure Storage advanced threat protection inschakelen](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-turn-on.png)

## <a name="explore-anomalies"></a>Verken onregelmatigheden

Wanneer storage afwijkingen in de activiteit is uitgevoerd, ontvangt u een e-mailmelding met informatie over de verdachte beveiligingsgebeurtenis. Details van de gebeurtenis zijn onder andere:

* aard van de anomaliedetectie
* De naam van opslagaccount
* Opslagtype
* tijd van de gebeurtenis

Het e-mailbericht ook vindt u informatie over mogelijke oorzaken en aanbevolen acties om te onderzoeken en oplossen van de mogelijke bedreiging.

![Azure Storage advanced threat protection e-mailwaarschuwing](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert-email.png)

U kunt bekijken en beheren van uw huidige beveiligingswaarschuwingen in Azure Security Center [tegel beveiligingswaarschuwingen](../../security-center/security-center-managing-and-responding-alerts.md#managing-security-alerts). Te klikken op een specifieke waarschuwing bevat details en acties voor de huidige bedreiging te onderzoeken en toekomstige bedreigingen-adressering.

![Azure Storage advanced threat protection e-mailwaarschuwing](./media/storage-advanced-threat-protection/storage-advanced-threat-protection-alert.png)

## <a name="protection-alerts"></a>Protection-waarschuwingen

Waarschuwingen worden gegenereerd door ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van storage-accounts. Deze gebeurtenissen kunnen de volgende waarschuwing is geactiveerd:

* **Toegang vanaf ongebruikelijke locatie**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot een opslagaccount. Bijvoorbeeld wanneer iemand toegang heeft tot een opslagaccount vanuit een ongebruikelijke geografische locatie. In sommige gevallen detecteert de waarschuwing een legitieme actie (een nieuwe toepassing of de onderhoudsbewerking-ontwikkelaars). In andere gevallen detecteert de waarschuwing een schadelijke actie (voormalig werknemer, externe aanvaller, enzovoort).

* **Ongebruikelijke gegevensextractie**: deze waarschuwing wordt geactiveerd wanneer er een wijziging in het patroon voor het extraheren van gegevens van een storage-account. Bijvoorbeeld, als iemand anders toegang gekregen een ongebruikelijke hoeveelheid gegevens in een storage-account tot heeft. In sommige gevallen detecteert de waarschuwing een legitieme actie (onderhoud activiteit). In andere gevallen detecteert de waarschuwing een schadelijke actie (exfiltratie/inbreuk op gegevens, niet-geautoriseerde overdracht van gegevens).

* **Ongebruikelijke anonieme toegang:** deze waarschuwing wordt geactiveerd wanneer er een wijziging in het toegangspatroon tot een opslagaccount. Stel bijvoorbeeld dat iemand anoniem toegang heeft gekregen tot een opslagaccount. In sommige gevallen detecteert de waarschuwing een legitieme toegang met behulp van openbare leestoegang. In andere gevallen detecteert de waarschuwing niet-geautoriseerde toegang die misbruik maakt van openbare leestoegang tot een container en de blobs.

* **Onverwachte verwijderen:** deze waarschuwing wordt geactiveerd wanneer een of meer onverwachte delete-bewerkingen in een storage-account plaatsvinden, op basis van historische analyse van de storage-account. Stel dat iemand uitgevoerd een *DeleteBlob* opnieuw met een nieuwe toepassing en naar een nieuw IP-adres. In sommige gevallen detecteert de waarschuwing een legitieme actie (de beheerder gebruikt een andere browser onderweg business). In andere gevallen detecteert de waarschuwing een schadelijke actie (een aanvaller het verwijderen van gegevens). 
 
* **Toegang tot de machtiging wijzigen:** deze waarschuwing wordt geactiveerd wanneer er een onverwachte wijziging van toegangsmachtigingen tot een opslagaccount. Stel bijvoorbeeld dat iemand de toegangsmachtigingen gewijzigd naar een opslagaccount met behulp van een nieuwe toepassing en van een nieuw IP-adres. In sommige gevallen detecteert de waarschuwing een legitieme actie (de beheerder gebruikt een andere browser onderweg business). In andere gevallen detecteert de waarschuwing een schadelijke actie (bijvoorbeeld een aanvaller de bevoegdheden van een account dat ze toegang tot hebben opgedaan verhogen). 

* **Uploaden van Azure Cloud Service-pakket:** deze waarschuwing wordt geactiveerd wanneer er een onverwachte uploaden van een Azure Cloud Service-pakket (*.cspkg* bestand) naar een opslagaccount. Stel een *.cspkg* bestand is geüpload van een nieuw IP-adres. In sommige gevallen detecteert de waarschuwing een legitieme actie. In andere gevallen detecteert de waarschuwing een schadelijke actie (bijvoorbeeld, een Cloudservice pakket is geüpload in voorbereiding voor een implementatie van een schadelijke service).    
   

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [registreert in Azure Storage-accounts ](/rest/api/storageservices/About-Storage-Analytics-Logging)

* Meer informatie over [Azure Security Center](../../security-center/security-center-intro.md)
---
title: Verbinding maken met Microsoft Advanced Threat Analytics met Azure Security Center | Microsoft Docs
description: Meer informatie over hoe Azure Security Center integreert met Microsoft Advanced Threat Analytics.
services: security-center
documentationcenter: na
author: YuriDio
manager: MBaldwin
editor: 
ms.assetid: 5d80bf91-16c3-40b3-82fc-e0805e6708db
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: e1b9e598af3b55c1d9591e5c1e529a80ae3319ca
ms.sourcegitcommit: ccb84f6b1d445d88b9870041c84cebd64fbdbc72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/14/2017
---
# <a name="connecting-microsoft-advanced-threat-analytics-to-azure-security-center"></a>Maken van verbinding Microsoft Advanced Threat Analytics met Azure Security Center
Dit document helpt u de integratie tussen Microsoft Advanced Threat Analytics en Azure Security Center configureren.

## <a name="why-add-advanced-threat-analytics-data"></a>Waarom is de toegevoegde gegevens Advanced Threat Analytics?
[Advanced Threat Analytics (ATA)](https://docs.microsoft.com/advanced-threat-analytics/what-is-ata) een on-premises-platform die helpt bij het detecteren van verdachte gedrag is. Wanneer verbinding is gemaakt, bent u verdachte acties die zijn gedetecteerd door ATA in Security Center weergeven. Deze integratie kunt u weergeven en onderzoeken van alle beveiligingswaarschuwingen die betrekking hebben op uw hybride cloud-werkbelastingen in Security Center correleren. 

## <a name="how-do-i-configure-this-integration"></a>Hoe kan ik deze integratie configureren?
Ervan uitgaande dat u hebt al ATA is geïnstalleerd, en goed werkt on-premises, als volgt te werk om deze integratie te configureren:

1. Meld u aan bij de ATA Center en toegang tot de ATA-portal.
2. Klik op **Syslog-server** in het linkerdeelvenster.

    ![Syslog-server](./media/security-center-ata-integration/security-center-ata-integration-fig1.png)

3. In de **Syslog-servereindpunt** opgeven, 127.0.0.7 (dit moet dit adres) en 5114 op de poort (aanbevolen). Terwijl het poortnummer aanbevolen wordt, moet een unieke poort werken. Laat alle andere opties is en klik op **opslaan**.
4. Klik op **meldingen** in het linkerdeelvenster en schakel alle Syslog-meldingen (aanbevolen), zoals wordt weergegeven in de volgende afbeelding:

    ![Meldingen](./media/security-center-ata-integration/security-center-ata-integration-fig2.png)

5. Klik op **Opslaan**.
6. Open het dashboard **Security Center**.
7. Klik in het linkerdeelvenster op **beveiligingsoplossingen**.
8. Onder **Advanced Threat Analytics**, klikt u op **toevoegen**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig3.png)
    
9. Ga naar de laatste stap en klik op **agent downloaden**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig4.png)

10. In de **nieuwe niet-Azure-computer toevoegen** pagina, selecteert u de werkruimte.

    ![Niet-Azure](./media/security-center-ata-integration/security-center-ata-integration-fig5.png)

11. In de **Direct Agent** pagina, de juiste Windows-agent downloaden en notities van de **werkruimte-ID** en **primaire sleutel**.

    ![Directe agent](./media/security-center-ata-integration/security-center-ata-integration-fig6.png)

12. Deze agent in het ATA Center installeren. Tijdens de installatie, zorg ervoor dat u de optie **Koppel de agent voor Azure logboekanalyse (OMS)**, en geef de *werkruimte-ID*, en *primaire sleutel* wanneer aangevraagd .


Zodra de installatie is voltooid, de integratie is voltooid en u zich voor een overzicht van nieuwe waarschuwingen verzonden van ATA naar Security Center in **beveiligingswaarschuwingen** en **Search**. De oplossing wordt weergegeven in de **beveiligingsoplossingen** pagina onder **verbonden oplossingen**. 

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Microsoft ATA Security Center. Zie de volgende artikelen voor meer informatie over Security Center:

* [Azure Active Directory Identity Protection koppelen aan Azure Security Center](security-center-aadip-integration.md)
* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md) : informatie over het beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen configureren.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) : Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Het beheren van en reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
- [Beveiliging van gegevens van Azure Security Center](security-center-data-security.md) -informatie over hoe gegevens worden beheerd en beveiligd in Security Center.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) — Lees het laatste nieuws van de Azure-beveiliging en de informatie.



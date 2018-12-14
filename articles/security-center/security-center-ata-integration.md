---
title: Microsoft Advanced Threat Analytics met Azure Security Center verbinden | Microsoft Docs
description: Meer informatie over hoe Azure Security Center kan worden geïntegreerd met Microsoft Advanced Threat Analytics.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 5d80bf91-16c3-40b3-82fc-e0805e6708db
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/29/2018
ms.author: rkarlin
ms.openlocfilehash: 49d6e2ded2ad4b2e5bb0ee30c323b1f459422437
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53338393"
---
# <a name="connecting-microsoft-advanced-threat-analytics-to-azure-security-center"></a>Maken van verbinding Microsoft Advanced Threat Analytics met Azure Security Center
Dit document helpt u bij het configureren van de integratie tussen Microsoft Advanced Threat Analytics en Azure Security Center.

## <a name="why-add-advanced-threat-analytics-data"></a>Advanced Threat Analytics-gegevens waarom toevoegen?
[Advanced Threat Analytics (ATA)](https://docs.microsoft.com/advanced-threat-analytics/what-is-ata) is een on-premises platform waarmee helpt bij het detecteren van verdachte gedrag. Wanneer de verbinding is gemaakt, bent u verdachte activiteit gedetecteerd door ATA in Security Center weergeven. Deze integratie kunt u bekijken, correleren en onderzoeken van alle beveiligingswaarschuwingen die betrekking hebben op uw hybride cloudworkloads in Security Center.

## <a name="how-do-i-configure-this-integration"></a>Hoe configureer ik deze integratie?
Ervan uitgaande dat u al hebt ATA is geïnstalleerd, en goed werkt on-premises, als volgt te werk om deze integratie te configureren:

1. Meld u aan bij de ATA Center en toegang tot de ATA-portal.
2. Klik op **Syslog-server** in het linkerdeelvenster.

    ![Syslog-server](./media/security-center-ata-integration/security-center-ata-integration-fig1.png)

3. In de **Syslog-servereindpunt** veld, typt u 127.0.0.7 (deze moet dit adres) en typ 5114 op de poort (aanbevolen). Terwijl het poortnummer dat aanbevolen wordt, alle unieke poort kan worden gebruikt. Laat alle andere opties, en klikt u op **opslaan**.
4. Klik op **meldingen** in het linkerdeelvenster en schakel alle Syslog-meldingen (aanbevolen), zoals wordt weergegeven in de volgende afbeelding:

    ![Meldingen](./media/security-center-ata-integration/security-center-ata-integration-fig2.png)

5. Klik op **Opslaan**.
6. Open het dashboard **Security Center**.
7. Klik in het linkerdeelvenster op **beveiligingsoplossingen**.
8. Onder **Advanced Threat Analytics**, klikt u op **toevoegen**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig3.png)

9. Ga naar de laatste stap en klik op **downloaden agent**.

    ![ATA](./media/security-center-ata-integration/security-center-ata-integration-fig4.png)

10. In de **nieuwe niet-Azure-computer toevoegen** pagina, selecteert u de werkruimte.

    ![Niet-Azure](./media/security-center-ata-integration/security-center-ata-integration-fig5.png)

11. In de **Direct Agent** pagina, de juiste Windows-agent downloaden en noteert de **werkruimte-ID** en **primaire sleutel**.

    ![Agent toewijzen](./media/security-center-ata-integration/security-center-ata-integration-fig6.png)

12. Deze agent in het ATA Center installeren. Tijdens de installatie, zorg ervoor dat u de optie **de agent verbinden met Azure Log Analytics**, en geef de *werkruimte-ID*, en *primaire sleutel* wanneer aangevraagd.


Zodra u klaar bent met de installatie, de integratie is voltooid en u wordt mogelijk nieuwe waarschuwingen die vanaf ATA verzonden naar Security Center in de **zoeken** resultaat. De oplossing wordt weergegeven in de **beveiligingsoplossingen** pagina onder **verbonden oplossingen**.

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u verbinding maken met Microsoft ATA met Security Center. Zie de volgende artikelen voor meer informatie over Security Center:

* [Azure Active Directory Identity Protection koppelen aan Azure Security Center](security-center-aadip-integration.md)
* [Beveiligingsbeleid instellen in Azure Security Center](tutorial-security-policy.md) : informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) : Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md): meer informatie over het bewaken van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](security-center-managing-and-responding-alerts.md) : informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
- [Beveiliging van Azure Security Center gegevens](security-center-data-security.md) -informatie over hoe gegevens worden beheerd en beveiligd in Security Center.
* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) : ophalen van de meest recente Azure-beveiliging-nieuws en informatie.

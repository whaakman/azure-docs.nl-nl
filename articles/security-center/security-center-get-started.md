---
title: Azure Security Center Quick Start - vrijgeven uw Azure-abonnement Security Center Standard | Microsoft Docs
description: Deze snelstartgids wordt beschreven hoe u een upgrade uitvoert naar de Security Center standaard prijscategorie voor extra beveiliging.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/07/2018
ms.author: terrylan
ms.openlocfilehash: ac4e3b36b08223f7e3b54850ed53a8185e85f0d2
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Snelstartgids: Vrijgeven uw Azure-abonnement Security Center Standard
Azure Security Center biedt uniform beheer en threat beveiliging via de werkbelasting van uw hybride cloud. Terwijl de laag gratis beperkte beveiliging voor uw Azure-resources alleen biedt, breidt de prijscategorie Standard deze mogelijkheden voor on-premises en andere clouds. Standard Security Center helpt u bij het vinden en beveiligingsproblemen oplossen, toepassen toegangs- en besturingselementen voor het blokkeren van schadelijke activiteiten, met behulp van de analyses en intelligence bedreigingen te detecteren en snel reageren als aangevallen. U kunt proberen Security Center Standard zonder kosten voor de eerste 60 dagen.

In dit artikel, moet u upgraden naar de prijscategorie Standard voor extra beveiliging en Microsoft Monitoring Agent installeren op uw virtuele machines om te controleren op beveiligingsproblemen en bedreigingen.

## <a name="prerequisites"></a>Vereisten
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

Als u wilt een abonnement upgraden naar de prijscategorie Standard, moet u zijn toegewezen de rol van eigenaar abonnement, abonnement bijdrager of beveiligings-beheerder.

## <a name="enable-your-azure-subscription"></a>Uw Azure-abonnement inschakelen

1. Meld u aan bij de [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. Ga naar het **Microsoft Azure**-menu en selecteer **Security Center**. **Security Center - overzicht** wordt geopend.

 ![Security Center-overzicht][2]

**Security Center – overzicht** voorziet in een uniform overzicht van de beveiligingsstatus van de werkbelasting van uw hybride cloud, zodat u om te detecteren en beoordelen van de beveiliging van uw werkbelastingen kunt herkennen en verhelpen van risico's. Security Center wordt automatisch geactiveerd van uw Azure-abonnementen niet eerder vrijgegeven door u of een ander abonnement gebruiker naar de laag gratis.

U kunt bekijken en de lijst met abonnementen filteren door te klikken op de **abonnementen** menu-item. Security Center gaat nu verder met de beoordeling van de beveiliging van deze abonnementen op beveiligingsproblemen identificeren. U kunt het beveiligingsbeleid wijzigen voor het aanpassen van de soorten beoordelingen. Een beveiligingsbeleid definieert de gewenste configuratie van uw workloads en helpt ervoor te zorgen dat aan de beveiligingsvereisten van het bedrijf of aan regelgeving wordt voldaan.

Binnen enkele minuten van Security Center de eerste keer wordt gestart, ziet u mogelijk:

- **Aanbevelingen** manieren om te verbeteren de beveiliging van uw Azure-abonnementen. Te klikken op de **aanbevelingen** tegel, een geprioriteerde lijst wordt gestart.
- Een inventaris van **Compute**, **Networking**, **opslag & gegevens**, en **toepassingen** resources die worden nu beoordeeld door Security Center samen met de beveiligingsstatus van elk.

Om te profiteren van Security Center, moet u de stappen hieronder om te upgraden naar de prijscategorie Standard en Microsoft Monitoring Agent installeren.

## <a name="upgrade-to-the-standard-tier"></a>Een upgrade uitvoert naar de prijscategorie Standard
Omwille van de Security Center snelstartgidsen en zelfstudies moet u upgraden naar de prijscategorie Standard. De eerste 60 dagen zijn gratis en kunt u terugkeren naar de laag gratis elk gewenst moment.

1. Selecteer onder het hoofdmenu Security Center **voorbereiding voor geavanceerde beveiliging**.

2. Onder **voorbereiding voor geavanceerde beveiliging**, Security Center een lijst met abonnementen en werkruimten die in aanmerking voor onboarding. Selecteer een abonnement in de lijst.

  ![Een abonnement selecteren][4]

3. **Beveiligingsbeleid** bevat informatie over de brongroepen die zijn opgenomen in het abonnement. **Prijzen** ook wordt geopend.
4. Onder **prijzen**, selecteer **standaard** upgrade vanaf gratis naar standaard en klik op **opslaan**.

  ![Selecteer standaard][5]

Nu dat u hebt bijgewerkt naar de prijscategorie Standard, hebt u toegang tot extra Security Center-functies, zoals **adaptieve toepassing besturingselementen**, **alleen bij het toegang in uitvoeringstijd VM**, **beveiliging waarschuwingen**, **dreiging intelligence**, **automation hulpmiddelen marketing en verkoop**, en meer. Houd er rekening mee beveiligingswaarschuwingen wordt alleen weergegeven wanneer het Beveiligingscentrum detecteert schadelijke activiteiten.

  ![Beveiligingswaarschuwingen][7]

## <a name="automate-data-collection"></a>Automatisch verzamelen van gegevens
Security Center verzamelt gegevens van uw Azure VM's en niet-Azure-computers om te controleren op beveiligingsproblemen en bedreigingen. Gegevens worden verzameld met behulp van Microsoft Monitoring Agent, die verschillende configuraties betrekking hebben op beveiliging en gebeurtenislogboeken van de machine leest en kopieert de gegevens naar de werkruimte voor analyse. Security Center maakt standaard een nieuwe werkruimte voor u.

Wanneer automatische inrichting is ingeschakeld, wordt Microsoft Monitoring Agent in Security Center geïnstalleerd op alle ondersteunde virtuele Azure-machines en nieuwe bestanden die zijn gemaakt. Automatische inrichting wordt sterk aanbevolen.

Inschakelen van automatische inrichting van de Microsoft Monitoring Agent:

1. Selecteer onder het hoofdmenu Security Center **beveiligingsbeleid**.
2. Selecteer het abonnement.
3. Onder **beveiligingsbeleid**, selecteer **gegevensverzameling**.
4. Onder **gegevensverzameling**, selecteer **op** automatische inrichting inschakelen.
5. Selecteer **Opslaan**.

  ![Automatische inrichting inschakelen][6]

Security Center kunt met deze nieuwe inzicht in uw Azure VM's bieden extra aanbevelingen met betrekking tot system bijwerken, status, OS beveiligingsconfiguraties, endpoint protection, evenals aanvullende beveiligingswaarschuwingen te genereren.

  ![Aanbevelingen][8]

## <a name="clean-up-resources"></a>Resources opschonen
Andere snelstartgidsen en zelfstudies in deze verzameling bouwen voort op deze snelstartgids. Als u van plan bent om door te gaan op werken met de volgende snelstartgidsen en zelfstudies, blijven de prijscategorie Standard uitvoeren en houden automatische inrichting is ingeschakeld. Als u niet wilt doorgaan of wilt terugkeren naar de laag gratis:

1. Terug naar het hoofdmenu Security Center en selecteer **beveiligingsbeleid**.
2. Selecteer het abonnement of het beleid dat u wilt terugkeren naar vrije. **Beveiligingsbeleid voor** wordt geopend.
3. Onder **beleid onderdelen**, selecteer **prijscategorie**.
4. Selecteer **vrije** abonnement te wijzigen van standaard laag naar de laag gratis.
5. Selecteer **Opslaan**.

Als u automatisch inrichten uitschakelen wilt:

1. Terug naar het hoofdmenu Security Center en selecteer **beveiligingsbeleid**.
2. Selecteer het abonnement dat u wilt automatisch inrichten uitschakelen.
3. Onder **beveiligingsbeleid – gegevensverzameling**, selecteer **uit** onder **Onboarding** automatisch inrichten uitschakelen.
4. Selecteer **Opslaan**.

>[!NOTE]
> Uitschakelen van automatische inrichting verwijdert geen Microsoft Monitoring Agent van de Azure VM's waarop de agent is ingericht. Uitschakelen van automatische inrichting limieten beveiligingsbewaking voor uw resources.
>

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids bijgewerkt naar de Standard-laag en de Microsoft Monitoring Agent voor het beveiligingsbeheer van de geïntegreerde en threat protection ingericht via de werkbelasting van uw hybride cloud. Voor meer informatie over hoe u met Security Center, blijven de Quick Start voor onboarding Windows-computers die zich on-premises en in andere clouds.

> [!div class="nextstepaction"]
> [Snelstartgids: Ingebouwde Windows-computers met Azure Security Center](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/onboarding.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png

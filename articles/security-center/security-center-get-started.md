---
title: 'Snelstartgids voor Azure Security Center: uw Azure-abonnement onboarden naar Security Center Standard | Microsoft Docs'
description: In deze snelstartgids wordt beschreven hoe u een upgrade uitvoert naar de prijscategorie Standard van Security Center voor extra beveiliging.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security-center
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: terrylan
ms.openlocfilehash: d10cef33ef0c325d41c9539107b9a4cab5e916d8
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37059851"
---
# <a name="quickstart-onboard-your-azure-subscription-to-security-center-standard"></a>Snelstartgids: uw Azure-abonnement onboarden voor Security Center Standard
Azure Security Center biedt geïntegreerd beveiligingsbeheer en bedreigingsbeveiliging voor uw verschillende hybride cloudworkloads. De Gratis laag biedt beperkte beveiliging voor alleen uw Azure-resources, maar de prijscategorie Standard biedt deze mogelijkheden ook voor on-premises en andere clouds. Standard van Security Center helpt u beveiligingsproblemen te vinden en op te lossen, toegangs- en toepassingsbesturingselementen toe te passen om schadelijke activiteiten te blokkeren, bedreigingen te detecteren met behulp van analyses en gegevens en snel te reageren bij aanvallen. U kunt Security Center Standard de eerste 60 dagen kosteloos proberen.

In dit artikel gaat u upgraden naar de prijscategorie Standard voor extra beveiliging en MMA installeren op uw virtuele machines om te controleren op beveiligingsproblemen en bedreigingen.

## <a name="prerequisites"></a>Vereisten
U moet over een abonnement op Microsoft Azure beschikken om met Security Center aan de slag te gaan. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis account](https://azure.microsoft.com/pricing/free-trial/).

Als u een abonnement wilt upgraden naar de prijscategorie Standard, moet de rol Abonnementhouder, Inzender of Beveiligingsbeheerder aan u zijn toegewezen.

## <a name="enable-your-azure-subscription"></a>Uw Azure-abonnement inschakelen

1. Meld u aan bij de [Azure Portal](https://azure.microsoft.com/features/azure-portal/).
2. Ga naar het **Microsoft Azure**-menu en selecteer **Security Center**. **Security Center - Overzicht** wordt geopend.

 ![Overzicht van Security Center][2]

**Security Center - overzicht** biedt een duidelijk overzicht van de beveiligingsstatus van uw hybride cloudworkloads, zodat u de beveiliging van uw workloads kunt bepalen en beoordelen en risico's kunt herkennen en verminderen. Security Center activeert automatisch de Azure-abonnementen naar de Gratis laag waarvoor u of een andere abonnementsgebruiker niet eerder onboarding hebt uitgevoerd.

U kunt de lijst met abonnementen bekijken en filteren door op het menu-item **Abonnementen** te klikken. Security Center begint met het beoordelen van de beveiliging van deze abonnementen om beveiligingsproblemen te identificeren. U kunt het beveiligingsbeleid wijzigen om de soorten evaluaties aan te passen. Een beveiligingsbeleid definieert de gewenste configuratie van uw workloads en helpt ervoor te zorgen dat aan de beveiligingsvereisten van het bedrijf of aan regelgeving wordt voldaan.

Binnen enkele minuten nadat u Security Center voor het eerst hebt gestart, ziet u mogelijk het volgende:

- **Aanbevelingen** voor manieren om de beveiliging van uw Azure-abonnementen te verbeteren. Als u op de tegel **Aanbevelingen** klikt, wordt er een gerangschikte lijst geopend.
- Een overzicht van de resources **Compute en apps**, **Netwerken**, **Gegevensbeveiliging** en **Identiteit en toegang** die nu worden beoordeeld in Security Center, samen met de beveiligingsstatus van elke resource.

Voltooi de stappen hieronder om naar de prijscategorie Standard te upgraden en de MMA te installeren om zo volledig van Security Center te profiteren.

## <a name="upgrade-to-the-standard-tier"></a>Upgraden naar de prijscategorie Standard
Voor de snelstartgidsen en zelfstudies van Security Center moet u upgraden naar de prijscategorie Standard. De eerste 60 dagen zijn gratis en u kunt op elk gewenst moment terugkeren naar de Gratis laag.

1. Selecteer in het hoofdmenu van Security Center de optie **Onboarding naar geavanceerde beveiliging**.

2. Security Center geeft onder **Onboarding naar geavanceerde beveiliging** een lijst weer met abonnementen en werkruimten die in aanmerking komen voor onboarding. Selecteer een abonnement in de lijst.

  ![Een abonnement selecteren][4]

3. **Beveiligingsbeleid** bevat informatie over de resourcegroepen die in het abonnement zijn opgenomen. **Prijzen** wordt ook geopend.
4. Selecteer onder **Prijzen** de optie **Standard** om van Gratis naar Standard te upgraden en klik op **Opslaan**.

  ![Standard selecteren][5]

Nu dat u een upgrade naar de prijscategorie Standard hebt uitgevoerd, hebt u toegang tot extra Security Center-functies, zoals **besturingselementen voor adaptieve toepassingen**, **Just-In-Time-VM-toegang**, **beveiligingswaarschuwingen**, **bedreigingsinformatie**, **playbooks voor automatiseren** en meer. Houd er rekening mee dat beveiligingswaarschuwingen alleen worden weergegeven wanneer Security Center schadelijke activiteiten detecteert.

  ![Beveiligingswaarschuwingen][7]

## <a name="automate-data-collection"></a>Gegevensverzameling automatiseren
Security Center verzamelt gegevens van uw Azure-VM's en niet-Azure-computers om te controleren op beveiligingsproblemen en bedreigingen. De gegevens worden verzameld met behulp van de MMA, die verschillende configuraties en gebeurtenislogboeken met betrekking tot beveiliging van de machine leest en de gegevens kopieert naar uw werkruimte voor analyse. Security Center maakt standaard een nieuwe werkruimte voor u.

Als automatisch inrichten is ingeschakeld, installeert Security Center de MMA op alle ondersteunde Azure-VM's en op nieuwe VM's. Automatische inrichting wordt sterk aanbevolen.

Automatische inrichting van de MMA inschakelen:

1. Selecteer in het hoofdmenu van Security Center de optie **Beveiligingsbeleid**.
2. Selecteer het abonnement.
3. Onder **Beveiligingsbeleid** selecteert u **Gegevensverzameling**.
4. Selecteer onder **Gegevensverzameling** **Aan** om automatische inrichting in te schakelen.
5. Selecteer **Opslaan**.

  ![Automatische inrichting inschakelen][6]

Security Center kan deze nieuwe inzichten in uw Azure-VM's gebruiken om extra aanbevelingen te doen met betrekking tot de updatestatus van het systeem, de beveiligingsconfiguraties van het besturingssysteem en de eindpuntbeveiliging. Ook kan Security Center aanvullende beveiligingswaarschuwingen genereren.

  ![Aanbevelingen][8]

## <a name="clean-up-resources"></a>Resources opschonen
Andere snelstartgidsen en zelfstudies in deze verzameling zijn gebaseerd op deze snelstartgids. Als u de volgende snelstartgidsen en zelfstudies ook wilt doornemen, blijf dan de prijscategorie Standard gebruiken en houd automatische inrichting ingeschakeld. Als u niet wilt doorgaan of wilt terugkeren naar de laag gratis:

1. Ga terug naar het hoofdmenu van Security Center en selecteer **Beveiligingsbeleid**.
2. Selecteer het abonnement of het beleid dat u wilt terugzetten op Gratis. **Beveiligingsbeleid** wordt geopend.
3. Selecteer onder **BELEIDSONDERDELEN** de optie **Prijscategorie**.
4. Selecteer **Gratis** om het abonnement te wijzigen van de Standard-laag in de Gratis laag.
5. Selecteer **Opslaan**.

Als u automatisch inrichten wilt uitschakelen:

1. Ga terug naar het hoofdmenu van Security Center en selecteer **Beveiligingsbeleid**.
2. Selecteer het abonnement waarvoor u automatisch inrichten wilt uitschakelen.
3. Ga naar **Beveiligingsbeleid – Gegevensverzameling** en selecteer onder **Onboarding** de optie **Uit** om automatisch inrichten uit te schakelen.
4. Selecteer **Opslaan**.

>[!NOTE]
> Wanneer u automatische inrichting uitschakelt, wordt MMA niet verwijderd van Azure-VM's waarop de agent is ingericht. Door automatische inrichting uit te schakelen, wordt de beveiligingsbewaking voor uw resources beperkt.
>

## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u een upgrade naar de prijscategorie Standard uitgevoerd en de MMA ingericht voor geïntegreerd beveiligingsbeheer en bescherming tegen bedreigingen voor uw hybride cloudworkloads. Ga naar de snelstartgids voor het onboarden van Windows-computers die zich on-premises en in andere clouds bevinden voor meer informatie over hoe u Security Center kunt gebruiken.

> [!div class="nextstepaction"]
> [Snelstartgids: Windows-computers onboarden naar Azure Security Center](quick-onboard-windows-computer.md)

<!--Image references-->
[2]: ./media/security-center-get-started/overview.png
[4]: ./media/security-center-get-started/onboarding.png
[5]: ./media/security-center-get-started/pricing.png
[6]: ./media/security-center-get-started/enable-automatic-provisioning.png
[7]: ./media/security-center-get-started/security-alerts.png
[8]: ./media/security-center-get-started/recommendations.png

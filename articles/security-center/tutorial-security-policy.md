---
title: Azure Security Center-zelfstudie - Beveiligingsbeleidsregels opstellen en beoordelen | Microsoft Docs
description: Azure Security Center-zelfstudie - Beveiligingsbeleidsregels opstellen en beoordelen
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 2d248817-ae97-4c10-8f5d-5c207a8019ea
ms.service: security-center
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/30/2018
ms.author: terrylan
ms.openlocfilehash: 15c69bce87ede96eb3a7bc0bada4e4f6a6669abb
ms.sourcegitcommit: 99a6a439886568c7ff65b9f73245d96a80a26d68
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39358605"
---
# <a name="tutorial-define-and-assess-security-policies"></a>Zelfstudie: Beveiligingsbeleidsregels opstellen en beoordelen
Security Center helpt ervoor te zorgen dat beveiligingsvereisten van het bedrijf en de overheid worden nageleefd, door middel van beveiligingsbeleidsregels waarmee de gewenste configuratie van uw workloads kunnen worden gedefinieerd. Wanneer u beleidsregels voor uw Azure-abonnementen definieert en deze aanpast aan het type workload of de vertrouwelijkheid van uw gegevens, kan Security Center beveiligingsaanbevelingen geven voor beveiliging van uw computer-, toepassings-, netwerk-, gegevens en opslag-, identiteit en toegangresources. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Beveiligingsbeleid configureren
> * De beveiliging van uw resources beoordelen

Als u nog geen Azure-abonnement hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Om de functies in deze zelfstudie te doorlopen, moet u zich in de Standard-prijscategorie van Security Center bevinden. U kunt Security Center Standard de eerste 60 dagen kosteloos proberen. In de snelstartgids [Onboard your Azure subscription to Security Center Standard](security-center-get-started.md) (Uw Azure-abonnement registreren voor Security Center Standard) wordt u begeleid bij het upgraden naar Standard.

## <a name="configure-security-policy"></a>Beveiligingsbeleid configureren
In Security Center wordt voor elk van uw Azure-abonnementen automatisch een standaardbeveiligingsbeleid gemaakt. Beveiligingsbeleidsregels bestaan uit aanbevelingen die u kunt in- of uitschakelen al naargelang de beveiligingsvereisten van dat abonnement. Alleen een eigenaar, bijdrager of beveiligingsbeheerder van het abonnement kan het standaardbeveiligingsbeleid wijzigen.

1. Selecteer in het hoofdmenu van Security Center de optie **Beveiligingsbeleid**.
2. Selecteer het abonnement dat u wilt gebruiken.

  ![Beveiligingsbeleid](./media/tutorial-security-policy/tutorial-security-policy-fig1.png)  

3. Selecteer bij **BELEIDSONDERDELEN** de optie **Beveiligingsbeleid**.
4. Selecteer **Aan** voor elke beveiligingsconfiguratie die u wilt bewaken. Security Center zal de configuratie van uw omgeving voortdurend bewaken, en wanneer er een beveiligingsprobleem is, wordt door Security Center een beveiligingsaanbeveling gegenereerd. Selecteer **Uit** als de beveiligingsconfiguratie niet aanbevolen of niet relevant is. Zo hebt u bijvoorbeeld in een ontwikkel-/testomgeving misschien niet hetzelfde beveiligingsniveau nodig als in een productie-omgeving. Klik op **Opslaan** nadat u de beleidsregels hebt geselecteerd die van toepassing zijn op uw omgeving.

  ![Beveiligingsconfiguratie](./media/tutorial-security-policy/tutorial-security-policy-fig6.png)  

Wacht totdat Security Center deze beleidsregels heeft verwerkt en aanbevelingen heeft gegenereerd. Sommige configuraties, zoals systeemupdates en OS-configuraties kunnen wel 12 uur duren. Netwerkbeveiligingsgroepen en versleutelingsconfiguraties kunnen daarentegen vrijwel onmiddellijk worden beoordeeld. Zodra u de aanbevelingen in het dashboard van Security Center ziet, kunt u doorgaan met de volgende stap.

## <a name="assess-security-of-resources"></a>Beveiliging van resources beoordelen
1. Op basis van de ingeschakelde beveiligingsbeleidsregels biedt Security Center indien nodig een reeks beveiligingsaanbevelingen. Controleer eerst de aanbevelingen voor virtuele machines en computers. Open het dashboard van **Security Center** en selecteer **Compute en apps**.

  ![Compute](./media/tutorial-security-policy/tutorial-security-policy-fig2.png)

  Controleer elke aanbeveling door de aanbevelingen in het rood (hoge prioriteit) de hoogste prioriteit te geven. Sommige van deze aanbevelingen bevatten herstel dat direct vanuit Security Center kan worden geïmplementeerd, zoals het [probleem met endpoint protection](https://docs.microsoft.com/azure/security-center/security-center-install-endpoint-protection). Andere aanbevelingen hebben alleen richtlijnen voor het toepassen van herstel, zoals de aanbeveling voor ontbrekende schijfversleuteling.

2. Wanneer u alle relevante compute-aanbevelingen hebt afgehandeld, gaat u verder met de volgende workload: netwerken. Open het dashboard van **Security Center** en klik op **Netwerken**.

  ![Netwerken](./media/tutorial-security-policy/tutorial-security-policy-fig3.png)

  De pagina met netwerkaanbevelingen bevat een lijst met beveiligingsproblemen op het gebied van de netwerkconfiguratie, internetgerichte eindpunten en netwerktopologie. Net als bij **Compute en apps** zullen enkele netwerkaanbevelingen geïntegreerd herstel bevatten en andere niet.

3. Wanneer u alle relevante netwerkaanbevelingen hebt afgehandeld, gaat u verder met de volgende workload: opslag en gegevens. Open het dashboard van **Security Center** en selecteer **Gegevens en opslag**.

  ![Gegevensbronnen](./media/tutorial-security-policy/tutorial-security-policy-fig4.png)

  De pagina **Gegevensbronnen** bevat aanbevelingen voor het inschakelen van controle-instellingen voor Azure SQL-servers en -databases, het inschakelen van versleuteling voor SQL-databases en het inschakelen van versleuteling van uw Azure Storage-account. Als u deze workloads niet hebt, wordt geen aanbeveling weergegeven. Net als bij **Compute en apps** zullen enkele gegevens- en opslagaanbevelingen geïntegreerd herstel bevatten en andere niet.

4. Wanneer u alle relevante gegevens- en opslagaanbevelingen hebt afgehandeld, gaat u verder met de volgende workload: Identiteit en toegang. Open het dashboard van Security Center, klik op **Overzicht** en vervolgens op **Identiteit en toegang**.

  ![Identiteit en toegang](./media/tutorial-security-policy/tutorial-security-policy-fig5.png)

  De pagina **Identiteit en toegang** bevat aanbevelingen, zoals:

   - MFA inschakelen voor bevoegde accounts voor uw abonnement
   - Externe accounts met schrijfmachtigingen verwijderen van uw abonnement
   - Bevoegde externe accounts verwijderen van uw abonnement

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
In deze zelfstudie hebt u geleerd over basisbeleidsdefinitie en beveiligingsbeoordeling van uw workload met Security Center, zoals:

> [!div class="checklist"]
> * Configuratie van het beveiligingsbeleid voor naleving van de beveiligingsvereisten van uw bedrijf of de overheid
> * Beveiligingsbeoordeling van compute-, netwerk-, SQL- en opslag- en toepassingsbronnen

Ga naar de volgende zelfstudie voor informatie over hoe u Security Center gebruikt om uw resources te beschermen.

> [!div class="nextstepaction"]
> [Uw resources beveiligen](tutorial-protect-resources.md)

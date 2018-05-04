---
title: 'Zelfstudie: toegang beheren in Azure Cost Management | Microsoft Docs'
description: In deze zelfstudie leest u hoe u de toegang tot gegevens van Cost Management kunt beheren met behulp van gebruikersaccounts die toegangsniveaus voor entiteiten bepalen.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 04/26/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: ''
manager: dougeby
ms.openlocfilehash: c1be4d649bf4b69a9f749003b5c66142006b78e0
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-assign-access-to-cost-management-data"></a>Zelfstudie: toegang tot gegevens van Cost Management beheren

De toegang tot gegevens van Cost Management wordt beheerd door middel van gebruikers of -entiteitsbeheer. Gebruikersaccounts van Cloudyn bepalen de toegang tot *entiteiten* en beheerfuncties. Er zijn twee soorten toegang: beheerder en gebruiker. Tenzij dit per gebruiker is gewijzigd, biedt beheerderstoegang gebruikers onbeperkte toegang tot alle functies in de portal van Cloudyn, met inbegrip van: beheer van gebruikers en lijsten met ontvangers, toegang als hoofdentiteit tot alle entiteitsgegevens. Gebruikerstoegang is bedoeld voor eindgebruikers om rapporten weer te geven en om rapporten te maken met de toegang die ze hebben tot entiteitsgegevens.

Entiteiten worden gebruikt om de hiërarchische structuur van de organisatie van uw bedrijf voor te stellen en vertegenwoordigen afdelingen, divisies en teams in uw organisatie in Cloudyn. De entiteitshiërarchie helpt u om nauwkeurig de uitgave bij te houden van de verschillende entiteiten.

Op het moment dat u uw Azure overeenkomst of -account registreert, wordt er een account met beheerdersmachtigingen gemaakt in Cloudyn. Met behulp van dit account kunt u alle stappen in deze zelfstudie uitvoeren. In deze zelfstudie wordt aandacht besteed aan toegang tot gegevens van Cost Management en entiteitsbeheer. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een gebruiker met beheerderstoegang maken
> * Een gebruiker met gebruikerstoegang maken
> * Entiteiten maken

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

## <a name="prerequisites"></a>Vereisten

- U moet een Azure-account hebben.
- U moet een proefregistratie of een betaald abonnement voor Azure Cost Management hebben.

## <a name="create-a-user-with-admin-access"></a>Een gebruiker met beheerderstoegang maken

Hoewel u al beschikt over beheerderstoegang, hebben collega's in uw organisatie misschien ook beheerderstoegang nodig. Klik in de rechterbovenhoek van de portal van Cloudyn op het tandwielpictogram en selecteer **User Management**. Klik op **Add New User** om een nieuwe gebruiker toe te voegen.

Voer de vereiste gegevens in voor de gebruiker. De aanmeldings-id moet een geldig e-mailadres zijn. Selecteer Allow User Management om het beheer van gebruikers toe te staan, zodat de gebruiker andere gebruikers kan maken en wijzigen. Met Adressenlijstbeheer kan de gebruiker adressenlijsten bewerken. Op het moment dat u **Notify user by email** selecteert, wordt er vanuit Cloudyn per e-mail een koppeling met aanmeldingsgegevens verzonden naar de gebruiker. Bij de eerste aanmelding stelt de gebruiker een wachtwoord in.

Onder **User has admin access** is de hoofdentiteit van uw organisatie geselecteerd. Laat dit zo en sla vervolgens de gebruikersgegevens op. Als u de hoofdentiteit selecteert, heeft de gebruiker niet alleen beheerdersmachtigingen voor de hoofdentiteit in de structuur, maar ook voor alle onderliggende entiteiten.  
  ![Een nieuwe gebruiker met beheerderstoegang toevoegen](.\media\tutorial-user-access\new-admin-access.png)

## <a name="create-a-user-with-user-access"></a>Een gebruiker met gebruikerstoegang maken
Meestal is het zo dat gebruikers die toegang nodig hebben tot gegevens van Cost Management zoals dashboards en rapporten, moeten beschikken over gebruikerstoegang om deze onderdelen weer te geven. Maak een nieuwe gebruiker die toegang heeft die vergelijkbaar is met de gebruiker die u hebt gemaakt met beheerderstoegang, maar met de volgende verschillen:

- Schakel **Allow User Management** en **Allow Recipient lists Management** uit, evenals het selectievakje Select all in de lijst **User has admin access**.
- Selecteer in de lijst **User has user access** de entiteiten waartoe de gebruiker toegang moet hebben.
- Indien nodig kunt u in de lijst ernaast ook beheerderstoegang geven tot bepaalde entiteiten.

![Een nieuwe gebruiker met gebruikerstoegang toevoegen](.\media\tutorial-user-access\new-user-access.png)

Als u een Engelstalige zelfstudievideo wilt bekijken over het toevoegen van gebruikers, gaat u naar [Adding Users to Azure Cost Management](https://youtu.be/Nzn7GLahx30).

## <a name="create-entities"></a>Entiteiten maken

Wanneer u de hiërarchie met kostenentiteiten definieert, wordt het aanbevolen om de structuur van uw organisatie te volgen.

Bepaal tijdens het samenstellen van de structuur hoe u de kosten van entiteiten wilt of moet toerekenen aan bedrijfsonderdelen, kostenplaatsen, omgevingen en verkoopafdelingen. De entiteitsstructuur in Cloudyn is flexibel vanwege de overname van entiteiten. Afzonderlijke abonnementen voor uw cloudaccounts worden gekoppeld aan specifieke entiteiten. Dit betekent dat entiteiten meerdere tenants kunnen hebben. Met behulp van entiteiten kunt u specifieke gebruikers alleen toegang geven tot hun segment van uw bedrijf. Hierdoor blijven gegevens geïsoleerd, zelfs in grote delen van een bedrijf zoals dochterondernemingen. Bovendien helpt gegevensisolatie bij governance.  

Op het moment dat u uw Azure overeenkomst of -account registreert bij Cloudyn, worden er allerlei gegevens naar uw Cloudyn-account gekopieerd. Denk hierbij aan gegevens van uw Azure-resources, zoals gegevens over het gebruik, de prestaties, facturering en tags uit uw abonnementen. De entiteitsstructuur moet u echter handmatig maken. Als u de registratie bij Azure Resource Manager hebt overgeslagen, zijn er alleen factureringsgegevens en enkele asset-rapporten beschikbaar in de portal van Cloudyn.

Klik in de Cloudyn-portal rechtsboven op het tandwielsymbool en selecteer **Cloud-accounts** . U begint met één entiteit (de hoofdentiteit of root) en gaat vervolgens de structuur van boven naar beneden uitwerken. Hier ziet u een voorbeeld van een entiteitshiërarchie die een goed beeld geeft van de opbouw van veel IT-organisaties als de structuur helemaal voltooid is:

![Entiteitsstructuur](.\media\tutorial-user-access\entity-tree.png)

Klik naast **Entities** op **Add Entity**. Voer gegevens in van de persoon of afdeling die u wilt toevoegen. De velden **Full Name** en **Email** hoeven niet overeen te komen met bestaande gebruikers. Als u een lijst met toegangsniveaus wilt weergeven, zoekt u in de Help naar *Adding an entity*.

![Entiteit toevoegen](.\media\tutorial-user-access\add-entity.png)

Sla de wijzigingen op met **Save** als u klaar bent.


Als u een Engelstalige zelfstudievideo wilt bekijken over het maken van een hiërarchie voor entiteitskosten, gaat u naar [Creating a Cost Entity Hierarchy in Azure Cost Management](https://youtu.be/dAd9G7u0FmU).

Als u een gebruiker bent van een Enterprise Agreement voor Azure, kunt u een video bekijken over het koppelen van accounts en abonnementen aan entiteiten. Ga hiervoor naar [Connecting to Azure Resource Manager with Azure Cost Management](https://youtu.be/oCIwvfBB6kk) (ook Engelstalig vooralsnog).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een gebruiker met beheerderstoegang maken
> * Een gebruiker met gebruikerstoegang maken
> * Entiteiten maken

Als u Azure Resource Manager API-toegang nog niet hebt ingeschakeld voor uw accounts, gaat u verder met het volgende artikel.

> [!div class="nextstepaction"]
> [Azure-abonnementen en -accounts activeren](activate-subs-accounts.md)

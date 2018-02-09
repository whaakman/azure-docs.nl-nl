---
title: Toegang beheren in Azure Cost Management | Microsoft Docs
description: Hier leest u hoe u de toegang tot gegevens van Cost Management kunt beheren met behulp van gebruikersaccounts die toegangsniveaus voor entiteiten bepalen.
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 01/30/2018
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: 89639e6d9c06edf110dc8432fbefe1fbfd1298c9
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="assign-access-to-cost-management-data"></a>Toegang tot gegevens van Cost Management beheren

De toegang tot gegevens van Cost Management wordt beheerd door middel van gebruikers of -entiteitsbeheer. Gebruikersaccounts van Cloudyn bepalen de toegang tot *entiteiten* en beheerfuncties. Er zijn twee soorten toegang: beheerder en gebruiker. Tenzij dit per gebruiker is gewijzigd, biedt beheerderstoegang gebruikers onbeperkte toegang tot alle functies in de portal van Cloudyn, met inbegrip van: beheer van gebruikers en lijsten met ontvangers, toegang als hoofdentiteit tot alle entiteitsgegevens. Gebruikerstoegang is bedoeld voor eindgebruikers om rapporten weer te geven en om rapporten te maken met de toegang die ze hebben tot entiteitsgegevens.

Entiteiten worden gebruikt om de hiërarchische structuur van de organisatie van uw bedrijf voor te stellen en vertegenwoordigen afdelingen, divisies en teams in uw organisatie in Cloudyn. De entiteitshiërarchie helpt u om nauwkeurig de uitgave bij te houden van de verschillende entiteiten.

Op het moment dat u uw Azure overeenkomst of -account registreert, wordt er een account met beheerdersmachtigingen gemaakt in Cloudyn. Met behulp van dit account kunt u alle stappen in deze zelfstudie uitvoeren. In deze zelfstudie wordt aandacht besteed aan toegang tot gegevens van Cost Management en entiteitsbeheer. Procedures voor:

> [!div class="checklist"]
> * Een gebruiker met beheerderstoegang maken
> * Een gebruiker met gebruikerstoegang maken
> * Entiteiten maken



## <a name="create-a-user-with-admin-access"></a>Een gebruiker met beheerderstoegang maken

Hoewel u al beschikt over beheerderstoegang, hebben collega's in uw organisatie misschien ook beheerderstoegang nodig. Klik in de rechterbovenhoek van de portal van Cloudyn op het tandwielpictogram en selecteer **User Management**. Klik op **Add New User** om een nieuwe gebruiker toe te voegen.

Voer de vereiste gegevens in voor de gebruiker. U kunt het wachtwoordveld leeg laten, zodat de gebruiker zelf een nieuw wachtwoord kan instellen bij de eerste aanmelding. Op het moment dat u **Notify user by email** selecteert, wordt er vanuit Cloudyn per e-mail een koppeling met aanmeldingsgegevens verzonden naar de gebruiker. Selecteer Allow User Management om het beheer van gebruikers toe te staan, zodat de gebruiker andere gebruikers kan maken en wijzigen. Selecteer Allow Recipient lists Management als de gebruiker lijsten met ontvangers mag bewerken.

Onder **User has admin access** is de hoofdentiteit van uw organisatie geselecteerd. Laat dit zo en sla vervolgens de gebruikersgegevens op. Als u de hoofdentiteit selecteert, heeft de gebruiker niet alleen beheerdersmachtigingen voor de hoofdentiteit in de structuur, maar ook voor alle onderliggende entiteiten.  
  ![Een nieuwe gebruiker met beheerderstoegang toevoegen](.\media\tutorial-user-access\new-admin-access.png)

## <a name="create-a-user-with-user-access"></a>Een gebruiker met gebruikerstoegang maken
Meestal is het zo dat gebruikers die toegang nodig hebben tot gegevens van Cost Management zoals dashboards en rapporten, moeten beschikken over gebruikerstoegang om deze onderdelen weer te geven. Maak een nieuwe gebruiker die toegang heeft die vergelijkbaar is met de gebruiker die u hebt gemaakt met beheerderstoegang, maar met de volgende verschillen:

- Schakel **Allow User Management** en **Allow Recipient lists Management** uit, evenals het selectievakje Select all in de lijst **User has admin access**.
- Selecteer in de lijst **User has user access** de entiteiten waartoe de gebruiker toegang moet hebben.
- Indien nodig kunt u in de lijst ernaast ook beheerderstoegang geven tot bepaalde entiteiten.

![Een nieuwe gebruiker met gebruikerstoegang toevoegen](.\media\tutorial-user-access\new-user-access.png)

Als u een Engelstalige zelfstudievideo wilt bekijken over het toevoegen van gebruikers, gaat u naar [Adding Users to Azure Cost Management Powered by Cloudyn](https://youtu.be/Nzn7GLahx30).

## <a name="create-entities"></a>Entiteiten maken

Wanneer u de hiërarchie met kostenentiteiten definieert, wordt het aanbevolen om de structuur van uw organisatie te volgen.

Bepaal tijdens het samenstellen van de structuur hoe u de kosten van entiteiten wilt of moet toerekenen aan bedrijfsonderdelen, kostenplaatsen, omgevingen en verkoopafdelingen. De entiteitsstructuur in Cloudyn is flexibel vanwege de overname van entiteiten. Afzonderlijke abonnementen voor uw cloudaccounts worden gekoppeld aan specifieke entiteiten. Dit betekent dat entiteiten meerdere tenants kunnen hebben. Met behulp van entiteiten kunt u specifieke gebruikers alleen toegang geven tot hun segment van uw bedrijf. Hierdoor blijven gegevens geïsoleerd, zelfs in grote delen van een bedrijf zoals dochterondernemingen. Bovendien helpt gegevensisolatie bij governance.  

Op het moment dat u uw Azure overeenkomst of -account registreert bij Cloudyn, worden er allerlei gegevens naar uw Cloudyn-account gekopieerd. Denk hierbij aan gegevens van uw Azure-resources, zoals gegevens over het gebruik, de prestaties, facturering en tags uit uw abonnementen. De entiteitsstructuur moet u echter handmatig maken. Als u de registratie bij Azure Resource Manager hebt overgeslagen, zijn er alleen factureringsgegevens en enkele asset-rapporten beschikbaar in de portal van Cloudyn.

Klik in de rechterbovenhoek van de portal van Cloudyn op **Settings** en selecteer **Cloud Accounts**. U begint met één entiteit (de hoofdentiteit of root) en gaat vervolgens de structuur van boven naar beneden uitwerken. Hier ziet u een voorbeeld van een entiteitshiërarchie die een goed beeld geeft van de opbouw van veel IT-organisaties als de structuur helemaal voltooid is:

![Entiteitsstructuur](.\media\tutorial-user-access\entity-tree.png)

Klik naast **Entities** op **Add Entity**. Voer gegevens in van de persoon of afdeling die u wilt toevoegen. De velden **Full Name** en **Email** hoeven niet overeen te komen met bestaande gebruikers. Als u een lijst met toegangsniveaus wilt weergeven, zoekt u in de Help naar *Adding an entity*.

![Entiteit toevoegen](.\media\tutorial-user-access\add-entity.png)

Sla de wijzigingen op met **Save** als u klaar bent.


Als u een Engelstalige zelfstudievideo wilt bekijken over het maken van een hiërarchie voor entiteitskosten, gaat u naar [Creating a Cost Entity Hierarchy in Azure Cost Management Powered by Cloudyn](https://youtu.be/dAd9G7u0FmU).

Als u een gebruiker bent van een Enterprise Agreement voor Azure, kunt u een video bekijken over het koppelen van accounts en abonnementen aan entiteiten. Ga hiervoor naar [Connecting to Azure Resource Manager with Azure Cost Management Powered by Cloudyn](https://youtu.be/oCIwvfBB6kk) (ook Engelstalig vooralsnog).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een gebruiker met beheerderstoegang maken
> * Een gebruiker met gebruikerstoegang maken
> * Entiteiten maken

Ga naar de volgende zelfstudie als u wilt weten hoe u uitgaven kunt voorspellen aan de hand van historische gegevens.

> [!div class="nextstepaction"]
> [Toekomstige uitgaven voorspellen](tutorial-forecast-spending.md)

---
title: Toewijzen van toegang in Azure kosten Management | Microsoft Docs
description: "Toegang tot de kosten van beheergegevens met gebruikersaccounts die toegangsniveaus voor entiteiten definiëren toewijzen."
services: cost-management
keywords: 
author: bandersmsft
ms.author: banders
ms.date: 10/11/2017
ms.topic: tutorial
ms.service: cost-management
ms.custom: mvc
manager: carmonm
ms.openlocfilehash: a42f3b51bf6d888d0d5602887ed317c6164391ef
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2017
---
# <a name="assign-access-to-cost-management-data"></a>Toegang tot beheergegevens kosten toewijzen

Toegang tot kostengegevens beheer wordt verzorgd door een gebruiker of -entiteitsbeheer. Toegang tot de gebruikersaccounts Cloudyn bepalen *entiteiten* en beheerfuncties. Er zijn twee soorten toegang: admin en de gebruiker. Tenzij per gebruiker wordt gewijzigd, beheerderstoegang kunt u een gebruiker onbeperkte gebruikmaken van alle functies in de portal Cloudyn, met inbegrip van: beheer van gebruikers, het beheren van de lijsten met geadresseerden en hoofdmap entiteit toegang tot alle entiteitsgegevens. Gebruikerstoegang is bestemd voor eindgebruikers rapporten weergeven en rapporten maken met de toegang hebben tot entiteitsgegevens.

Entiteiten worden gebruikt in overeenstemming met de hiërarchische structuur van de bedrijfsorganisatie van uw. Ze identificeren afdelingen, afdelingen en teams in uw organisatie in Cloudyn. De entiteitshiërarchie kunt u nauwkeurige Houd uitgave door de entiteiten.

Nadat u uw Azure overeenkomst of account geregistreerd, wordt een account met machtigingen van de beheerder is gemaakt in Cloudyn, zodat u alle stappen in deze zelfstudie kunt uitvoeren. Deze zelfstudie bevat informatie over de toegang voor beheergegevens, met inbegrip van beheer van gebruikers en -entiteitsbeheer kosten. Procedures voor:

> [!div class="checklist"]
> * Een gebruiker met beheerderstoegang maken
> * Een gebruiker met toegang van gebruikers maken
> * Entiteiten maken



## <a name="create-a-user-with-admin-access"></a>Een gebruiker met beheerderstoegang maken

Hoewel u beheerderstoegang al hebt, wellicht collega's in uw organisatie ook beheerderstoegang hebben. Klik op het symbool tandwielpictogram in het bovenste rechts en selecteer in de portal Cloudyn **Gebruikersbeheer**. Klik op **nieuwe gebruiker toevoegen** een nieuwe gebruiker toe te voegen.

Voer de vereiste informatie over de gebruiker. U kunt het wachtwoordveld leeg laten zodat de gebruiker een nieuw wachtwoord op de eerste aanmelding instellen kan. Een koppeling met een teken in de informatie verzonden naar de gebruiker via e-mail van Cloudyn wanneer u selecteert **gebruiker waarschuwen via e-mail**. Kies de machtigingen voor het beheer van gebruikers toestaan, zodat de gebruiker kunt maken en wijzigen van andere gebruikers. Een lijst met beheer van geadresseerden zodat de gebruiker geadresseerde lijsten bewerken.

Onder **gebruiker heeft beheerderstoegang**, de entiteit van de hoofdmap van uw organisatie is geselecteerd. Laat hoofdmap geselecteerd en slaat u de gebruikersinformatie. De entiteit hoofdmap selecteren, kan de gebruiker gemachtigd admin niet alleen naar de hoofdmap entiteit in de structuur, maar ook op alle entiteiten die onder deze staan.  
  ![nieuwe gebruiker toevoegen met beheerderstoegang](.\media\tutorial-user-access\new-admin-access.png)

## <a name="create-a-user-with-user-access"></a>Een gebruiker met toegang van gebruikers maken
Typische gebruikers die toegang nodig tot kostengegevens management zoals dashboards en rapporten moeten beschikken over gebruiker weer te geven. Maak een nieuwe gebruiker met gebruikerstoegang vergelijkbaar zijn met die u hebt gemaakt met beheerderstoegang met de volgende verschillen:

- Schakel **Gebruikersbeheer toestaan**, **toestaan ontvanger geeft een lijst Management**, en wis alle de **gebruiker heeft beheerderstoegang** lijst.
- Selecteer de entiteiten die de gebruiker heeft toegang tot in de **gebruiker gebruikerstoegang heeft** lijst.
- U kunt ook een beheerder zijn om de toegang tot specifieke entiteiten toestaan indien nodig.

![nieuwe gebruiker toevoegen met gebruikerstoegang](.\media\tutorial-user-access\new-user-access.png)

Een zelfstudie als video wilt bekijken over het toevoegen van gebruikers, Zie [gebruikers toe te voegen aan Azure kosten Management Cloudyn](https://youtu.be/Nzn7GLahx30).

## <a name="create-entities"></a>Entiteiten maken

Wanneer u uw kosten entiteitenhiërarchie definieert, is een best practice om te identificeren van de structuur van uw organisatie.

Tijdens het samenstellen van de structuur, houd rekening met hoe moet of wilt u voor een overzicht van hun kosten Partitiespecifieke business units kostenplaatsen, omgevingen en verkoopafdeling. De structuur van de entiteit in Cloudyn is flexibel vanwege entiteit overname. Afzonderlijke abonnementen voor uw cloud-accounts zijn gekoppeld aan specifieke entiteiten. Entiteiten zijn dus meerdere tenants. U kunt specifieke gebruikers toegang toewijzen aan alleen hun segment van uw bedrijf met behulp van entiteiten. In dat geval blijven gegevens geïsoleerd, zelfs in grote delen van een bedrijf zoals dochterondernemingen. En gegevensisolatie helpt bij het toezicht.  

Nadat u uw account of Azure-overeenkomst met Cloudyn geregistreerd, is uw Azure-resource-gegevens met informatie over het gebruik, prestaties, facturering en labelgegevens van uw abonnementen aan uw account Cloudyn gekopieerd. U moet de structuur van uw entiteit echter handmatig maken. Als u de registratie van de Azure Resource Manager overgeslagen, klikt u vervolgens alleen factureringsgegevens en enkele asset rapporten zijn beschikbaar in de portal Cloudyn.

Klik in de portal Cloudyn **instellingen** in de linkerbovenhoek rechts en selecteer **Accounts in de Cloud**. U begint met een enkele entiteit (root) en bouwen van uw entiteit structuur uit onder de hoofdmap. Hier volgt een voorbeeld van de entiteitenhiërarchie van een die veel IT-organisaties mogelijk lijken op nadat de structuur voltooid is:

![structuur van de entiteit](.\media\tutorial-user-access\entity-tree.png)

Naast **entiteiten**, klikt u op **entiteit toevoegen**. Geef informatie op over de persoon of dienst die u wilt toevoegen. De **volledige naam** en **e** velden niet moeten overeenkomen met bestaande gebruikers. Als u een lijst weergeven met toegangsniveaus wilt, zoekt u in de help voor *toevoegen van een entiteit*.

![Entiteit toevoegen](.\media\tutorial-user-access\add-entity.png)

Wanneer u bent klaar, **opslaan** de entiteit.


Een zelfstudie als video wilt bekijken over het maken van een hiërarchie van de entiteit kosten, Zie [maken van een hiërarchie van de entiteit kosten in Azure kosten Management Cloudyn](https://youtu.be/dAd9G7u0FmU).

Als u een Enterprise Agreement voor Azure-gebruiker bent, bekijk de video van een zelfstudie over het koppelen van accounts en -abonnementen op entiteiten op [verbinding te maken in Azure Resource Manager met Azure kosten Management door Cloudyn](https://youtu.be/oCIwvfBB6kk).

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een gebruiker met beheerderstoegang maken
> * Een gebruiker met toegang van gebruikers maken
> * Entiteiten maken

Ga naar de volgende zelfstudie voor informatie over het voorspellen van uitgaven met behulp van historische gegevens.

> [!div class="nextstepaction"]
> [Toekomstige uitgaven voorspellen](tutorial-forecast-spending.md)

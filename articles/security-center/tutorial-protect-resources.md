---
title: Zelfstudie voor Azure Security Center - beveiligen van uw resources met Azure Security Center | Microsoft Docs
description: Deze zelfstudie leert u hoe u configureert een alleen toegang tot beleid en een Toepassingsbeheerbeleid tijdig VM.
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
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: f0a32f90e68101f805a52427fab2d5bb29b94939
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="tutorial-protect-your-resources-with-azure-security-center"></a>Zelfstudie: Beveiligen van uw resources met Azure Security Center
Security Center beperkt de blootstelling aan bedreigingen met behulp van besturingselementen voor toegang en de toepassing om te blokkeren van schadelijke activiteiten. Alleen bij tijd virtuele machine (VM) vermindert toegang tot uw blootgesteld aan aanvallen doordat u permanente toegang weigeren aan virtuele machines. U kunt in plaats daarvan beheerd en gecontroleerd toegang verlenen tot virtuele machines alleen wanneer deze nodig is. Adaptieve toepassing besturingselementen helpen bij het beperken van virtuele machines tegen schadelijke software te controleren welke toepassingen op uw virtuele machines kunnen uitvoeren. Security Center maakt gebruik van machine learning om de processen te analyseren die op de virtuele machine worden uitgevoerd. Ook helpt het u op basis van deze informatie regels voor opname in de whitelist toe te passen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Configureer een tijdstip VM toegangsbeleid
> * Configureer een Toepassingsbeheerbeleid

Als u geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) voordat u begint.

## <a name="prerequisites"></a>Vereisten
Als u wilt de functies behandeld in deze zelfstudie doorloopt, moet u op de Standard-prijscategorie van Security Center. U kunt proberen Security Center Standard zonder kosten voor de eerste 60 dagen. De Quick Start [Onboard uw Azure-abonnement Security Center Standard](security-center-get-started.md) wordt u begeleid bij het upgraden naar Standard.

## <a name="manage-vm-access"></a>VM-toegang beheren
In de tijd kan de virtuele machine toegang vergrendelen binnenkomend verkeer naar uw Azure VM's, blootstelling aan aanvallen te verminderen terwijl er eenvoudig toegang krijgen tot het verbinding maken met virtuele machines wanneer deze nodig worden gebruikt.

In de tijd is VM toegang preview.

Poorten voor beheertaken hoeft niet te allen tijde moeten worden gebruikt. Ze hoeft alleen te worden gebruikt terwijl u bent verbonden met de virtuele machine, zoals beheer- of taken uit te voeren. Wanneer in de tijd is ingeschakeld, Security Center Netwerkbeveiligingsgroep (NSG) regels, die toegang tot beheerpoorten beperken zodat ze niet worden gericht door aanvallers worden gebruikt.

1. Selecteer in het hoofdmenu Security Center **Just in time VM toegang** onder **geavanceerde CLOUD VERDEDIGING**.

  ![Just-In-Time-VM-toegang][1]

  **Alleen bij het toegang in uitvoeringstijd VM** bevat informatie over de status van uw virtuele machines:

  - **Geconfigureerd** -virtuele machines die zijn geconfigureerd om te ondersteunen alleen bij het VM-time-toegang.
  - **Aanbevolen** -VM's die alleen bij het toegang in uitvoeringstijd VM kunnen ondersteunen, maar niet is geconfigureerd voor.
  - **Geen aanbeveling** -redenen die kunnen leiden tot een virtuele machine niet aan te bevelen zijn:

    - NSG - de zojuist ontbreekt in de tijd oplossing vereist een NSG aan aanwezig zijn.
    - Klassieke VM - Security Center alleen bij het VM-time-toegang ondersteunt momenteel alleen virtuele machines die zijn geïmplementeerd via Azure Resource Manager.
    - Andere - een virtuele machine is in deze categorie als de zojuist tijdig oplossing is uitgeschakeld in het beveiligingsbeleid van het abonnement of de resourcegroep, of dat ontbreekt een openbaar IP-adres en geen een NSG in plaats van de virtuele machine.

2. Selecteer een aanbevolen virtuele machine en klik op **JIT inschakelen op 1 VM** voor het configureren van een in tijd beleid voor die VM:

  U kunt de standaard opslaan poorten die door Security Center raadt of u kunt toevoegen en configureren van een nieuwe poort waarop u wilt de zojuist inschakelen in tijdoplossing. In deze zelfstudie gaan we een poort toevoegen door te selecteren **toevoegen**.

  ![Poortconfiguratie toevoegen][2]

3. Onder **toevoegen poortconfiguratie**, u identificeren:

  - De poort
  - Het protocoltype
  - Bron-IP-adressen - toegestane IP-adresbereiken mag krijgen toegang op een verzoek goedgekeurd
  - Maximale aanvraagtijd - maximale tijdsduur dat een specifieke poort kan worden geopend

4. Selecteer **OK** om op te slaan.

## <a name="harden-vms-against-malware"></a>Virtuele machines beschermen tegen schadelijke software
Adaptieve toepassing beveiligingsmaatregelen helpen u bij het definiëren van een set toepassingen die mogen worden uitgevoerd op de geconfigureerde resourcegroepen, waarin onder andere voordelen helpt uw virtuele machines tegen schadelijke software te beperken. Security Center maakt gebruik van machine learning om de processen te analyseren die op de virtuele machine worden uitgevoerd. Ook helpt het u op basis van deze informatie regels voor opname in de whitelist toe te passen.

Besturingselementen voor adaptieve toepassing is een Preview-versie. Deze functie is alleen beschikbaar voor Windows-machines.

1. Ga terug naar het hoofdmenu Security Center. Onder **geavanceerde CLOUD VERDEDIGING**, selecteer **adaptieve toepassing besturingselementen**.

   ![Adaptieve toepassingsbesturingselementen][3]

  De **resourcegroepen** sectie bevat drie tabbladen:

  - **Geconfigureerd**: lijst met resource groepen waarin de virtuele machines die zijn geconfigureerd met Toepassingsbeheer.
  - **Aanbevolen**: lijst met resourcegroepen voor welke toepassing besturingselement wordt aanbevolen.
  - **Geen aanbeveling**: lijst met resource groepen met virtuele machines zonder toepassing besturingselement aanbevelingen. Hierbij kan het bijvoorbeeld gaan om virtuele machines waarop toepassingen steeds wisselen en geen stabiele status hebben.

2. Selecteer de **aanbevolen** tabblad voor een lijst met resourcegroepen met aanbevelingen voor het beheer van toepassing.

  ![Aanbevelingen voor het beheer van toepassing][4]

3. Selecteer een resourcegroep openen de **maken van regels voor toepassing** optie. Bekijk in **VM's selecteren** de lijst met aanbevolen virtuele machines en schakel alle virtuele machines uit waarop u geen toepassingsbeheer wilt toepassen. Bekijk in **Processen selecteren voor de regels voor opname in de whitelist** de lijst met aanbevolen toepassingen en schakel alle toepassingen uit waarop u geen toepassingsbeheer wilt toepassen. De lijst bevat:

  - **NAAM**: pad naar de volledige toepassing
  - **PROCESSEN**: hoeveel toepassingen zich bevinden binnen elke pad
  - **ALGEMENE**: 'Ja' geeft aan dat deze processen zijn uitgevoerd op de meeste VM's in deze resourcegroep
  - **MISBRUIK**: een waarschuwingspictogram geeft aan of de toepassingen kunnen worden gebruikt door een aanvaller toepassing whitelisting overslaan. U wordt aanbevolen om deze toepassingen te controleren voordat ze worden goedgekeurd.

4. Nadat u uw selecties hebt, selecteert u **maken**.

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
In deze zelfstudie hebt u geleerd hoe u uw blootstelling aan bedreigingen door beperken:

> [!div class="checklist"]
> * Configureren van een in tijd VM toegang beleid om te bieden beheerd en gecontroleerd toegang tot virtuele machines alleen wanneer deze nodig is
> * Een beleid adaptieve toepassing besturingselementen om te bepalen welke toepassingen kunnen worden uitgevoerd op uw virtuele machines configureren

Ga naar de volgende zelfstudie voor meer informatie over het reageren op beveiligingsincidenten.

> [!div class="nextstepaction"]
> [Zelfstudie: Reageren op beveiligingsincidenten](tutorial-security-incident.md)

<!--Image references-->
[1]: ./media/tutorial-protect-resources/just-in-time-vm-access.png
[2]: ./media/tutorial-protect-resources/add-port.png
[3]: ./media/tutorial-protect-resources/adaptive-application-control-options.png
[4]: ./media/tutorial-protect-resources/recommended-resource-groups.png

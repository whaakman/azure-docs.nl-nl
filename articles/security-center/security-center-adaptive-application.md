---
title: Adaptieve toepassingsbesturingselementen in Azure Security Center | Microsoft Docs
description: In dit document leert u hoe u adaptieve toepassingsbesturingselementen in Azure Security Center gebruikt om toepassingen die op virtuele Azure-machines worden uitgevoerd, in de whitelist op te nemen.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/20/2017
ms.author: yurid
ms.openlocfilehash: 9c3a9a7255bbbdab8f4c356eb07022d7f1d242d7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="adaptive-application-controls-in-azure-security-center-preview"></a>Adaptieve toepassingsbesturingselementen in Azure Security Center (preview)
Lees hoe u toepassingsbeheer configureert in Azure Security Center met behulp van dit stapsgewijze overzicht.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Wat zijn adaptieve toepassingsbesturingselementen in Security Center?
Adaptieve toepassingsbesturingselementen helpen bepalen welke toepassingen kunnen worden uitgevoerd op uw virtuele machines die zich in Azure bevinden. Dit biedt u diverse voordelen, waaronder de mogelijkheid om uw virtuele machines beter bestand te maken tegen schadelijke software. Security Center maakt gebruik van machine learning om de processen te analyseren die op de virtuele machine worden uitgevoerd. Ook helpt het u op basis van deze informatie regels voor opname in de whitelist toe te passen. Met deze mogelijkheid wordt het configureren en onderhouden van whitelists aanzienlijk eenvoudiger. U kunt hiermee het volgende:

- Pogingen om schadelijke toepassingen uit te voeren, blokkeren of hier een waarschuwing over verzenden, waaronder pogingen die anders mogelijk worden gemist door antimalwareoplossingen.
- Voldoen aan het beveiligingsbeleid van uw organisatie waarin alleen het gebruik van gelicentieerde software is toegestaan.
- Vermijden dat in uw omgeving ongewenste software wordt gebruikt.
- Vermijden dat oude en niet-ondersteunde apps worden uitgevoerd.
- Voorkomen dat bepaalde softwareprogramma's worden uitgevoerd die in uw organisatie niet zijn toegestaan.
- De IT-afdeling in staat stellen de toegang tot gevoelige gegevens te beheren via het gebruik van apps.

> [!NOTE]
> Adaptieve toepassingsbesturingselementen zijn als beperkte openbare preview beschikbaar voor klanten met Azure Security Center Standard. [Stuur ons een e-mail](mailto:ASC_appcontrol@microsoft.com) met uw abonnement-id's om deel te nemen aan de preview.

## <a name="how-to-enable-adaptive-application-controls"></a>Hoe worden adaptieve toepassingsbesturingselementen ingeschakeld?
Met adaptieve toepassingsbesturingselementen kunt u een set toepassingen definiëren die in de geconfigureerde resourcegroepen mogen worden uitgevoerd. Deze functie is alleen beschikbaar voor Windows-machines (alle versies, klassieke implementatie of Azure Resource Manager-implementatie). Volg de onderstaande stappen om opname in de whitelist met toepassingen in Security Center te configureren:

1.  Open het **Security Center**-dashboard en klik op **Overzicht**.
2.  Onder **Geavanceerde cloudbeveiliging** geeft de tegel **Adaptieve toepassingsbesturingselementen** aan voor hoeveel virtuele machines het besturingselement momenteel actief is in vergelijking met alle virtuele machines. Ook geeft de tegel het aantal problemen weer dat de afgelopen week is gevonden: 

    ![Adaptieve toepassingsbesturingselementen](./media/security-center-adaptive-application\security-center-adaptive-application-fig1.png)

3. Klik op de tegel **Adaptieve toepassingsbesturingselementen** voor meer opties.

    ![besturingselementen](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

4. De sectie Resourcegroepen bevat drie tabbladen:
    * **Aanbevolen**: een lijst met resourcegroepen waarvoor toepassingsbeheer wordt aanbevolen. Security Center maakt gebruik van machine learning om virtuele machines te identifiveren die baat zouden hebben bij het gebruik van toepassingsbeheer. Hierbij wordt gekeken of de virtuele machines consequent dezelfde toepassingen uitvoeren.
    * **Geconfigureerd**: een lijst met resourcegroepen met virtuele machines waarvoor toepassingsbeheer is geconfigureerd. 
    * **Geen aanbeveling**: een lijst met resourcegroepen met virtuele machines waarvoor geen aanbevelingen zijn gedaan voor het gebruik van toepassingsbeheer. Hierbij kan het bijvoorbeeld gaan om virtuele machines waarop toepassingen steeds wisselen en geen stabiele status hebben.

In de volgende secties worden alle opties en het gebruik ervan gedetailleerder besproken.

### <a name="configure-a-new-application-control-policy"></a>Een nieuw toepassingsbeheerbeleid configureren
Klik op het tabblad **Aanbevolen** voor een lijst met resourcegroepen waarvoor toepassingsbeheer wordt aanbevolen:

![Aanbevolen](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

De lijst bevat:
- **NAAM**: de naam van het abonnement en de resourcegroep
- **VM’s**: het aantal virtuele machines in de resourcegroep
- **STATUS**: de status van de aanbevelingen (in de meeste gevallen 'open')
- **ERNST**: de ernst van de aanbevelingen

Selecteer een resourcegroep om de optie **Maken van regels voor toepassingsbeheer** te openen:

![Regels voor toepassingsbeheer](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

Bekijk in **VM's selecteren** de lijst met aanbevolen virtuele machines en schakel alle virtuele machines uit waarop u geen toepassingsbeheer wilt toepassen. Bekijk in **Processen selecteren voor de regels voor opname in de whitelist** de lijst met aanbevolen toepassingen en schakel alle toepassingen uit waarop u geen toepassingsbeheer wilt toepassen. De lijst bevat:

- **NAAM**: het volledige toepassingspad
- **PROCESSEN**: hoeveel toepassingen zich binnen elk pad bevinden
- **ALGEMEEN**: 'waar' geeft aan dat deze processen zijn uitgevoerd op de meeste virtuele machines in deze resourcegroep.
- **EXPLOITEERBAAR**: er wordt een waarschuwingspictogram weergegeven als de toepassingen door een aanvaller kunnen worden gebruikt om opname in de whitelist met toepassingen te omzeilen. Het is raadzaam deze toepassingen te controleren voordat u ze goedkeurt. 

Klik op de knop **Maken** nadat u uw selecties hebt gemaakt. Standaard schakelt Security Center toepassingsbeheer altijd in de modus *Controle* in. Nadat is gevalideerd dat de whitelist geen negatieve gevolgen heeft voor uw workload, kunt u overschakelen naar de modus *Afdwingen*.

> [!NOTE]
> Als best practice voor beveiliging probeert Security Center standaard altijd een uitgeverregel te maken voor de toepassingen die in de whitelist moeten worden opgenomen. Alleen als een toepassing niet de gegevens van een uitgever bevat (ofwel niet is ondertekend), wordt er een padregel gemaakt voor het volledige pad van de specifieke EXE.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Een groep die met toepassingsbeheer is geconfigureerd, bewerken en bewaken

Klik op **GECONFIGUREERD** onder **Resourcegroepen** om een groep die met toepassingsbeheer is geconfigureerd, te bewerken en te bewaken:

![Resourcegroepen](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

De lijst bevat:

- **NAAM**: de naam van het abonnement en de resourcegroep
- **VM’s**: het aantal virtuele machines in de resourcegroep
- **MODUS**: de controlemodus legt pogingen tot uitvoering van niet in de whitelist opgenomen toepassingen vast; met Blokkeren wordt niet toegestaan dat niet in de whitelist opgenomen toepassingen worden uitgevoerd
- **ERNST**: de ernst van de aanbevelingen

Selecteer een resourcegroep om wijzigingen in aan te brengen op de pagina **Toepassingsbeheerbeleid bewerken**.

![Beveiliging](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

Onder **Beveiligingsmodus** kunt u een keuze maken uit de volgende opties:
- **Controle**: in deze modus worden de regels niet afgedwongen door de oplossing voor toepassingsbeheer en wordt alleen de activiteit op de beveiligde virtuele machines gecontroleerd. Dit wordt aanbevolen voor scenario's waarin u eerst het algehele gedrag wilt observeren voordat de uitvoering van een app op de doel-VM wordt geblokkeerd.
- **Afdwingen**: in deze modus worden de regels afgedwongen door de oplossing voor toepassingsbeheer. Ook worden toepassingen die niet mogen worden uitgevoerd, geblokkeerd. 

Zoals eerder vermeld, wordt een nieuw toepassingsbeheerbeleid altijd standaard geconfigureerd in de modus *Controle*. Onder **Beleidsuitbreiding** kunt u uw eigen toepassingspaden toevoegen die u in de whitelist wilt opnemen. Nadat u deze paden hebt toegevoegd, maakt Security Center de juiste regels voor deze toepassingen, naast de regels die al aanwezig zijn. In de sectie **Problemen** worden eventuele overtredingen vermeld.

![Problemen](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

Deze lijst bevat:

- **Problemen**: eventuele overtredingen die zijn geregistreerd, waaronder de volgende:
    - **ViolationsBlocked**: wanneer de oplossing in de modus Afdwingen is ingesteld en er wordt geprobeerd een toepassing uit te voeren die niet in de whitelist is opgenomen.
    - **ViolationsAudited**: wanneer de oplossing in de modus Controle is ingesteld en er een toepassing die niet in de whitelist is opgenomen, wordt uitgevoerd.
    - **RulesViolatedManually**: wanneer een gebruiker heeft geprobeerd handmatig regels te configureren op de virtuele machines en niet via de ASC-beheerportal.
- **AANTAL VM’s**: het aantal virtuele machines met dit type probleem.

Als u op elk van deze regels klikt, wordt u doorgestuurd naar de pagina [Azure Activity Log](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), waar u informatie over alle virtuele machines met dit type overtreding kunt zien. Als u op de drie punten aan het einde van elke regel klikt, kunt u die specifieke vermelding verwijderen. In de sectie **Geconfigureerde virtuele machines** worden de virtuele machines weergegeven waarop deze regels van toepassing zijn. 

![Geconfigureerde virtuele machines](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

In **Uitgeverregels voor opname in de whitelist** worden de toepassingen vermeld waarvoor een uitgeverregel is gemaakt op basis van de gegevens van het certificaat dat voor elke toepassing is gevonden. Zie [Uitgeverregels in Applocker](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker) voor meer informatie.

![Regels voor opname in de whitelist](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

Als u op de drie punten aan het einde van elke regel klikt, kunt u die specifieke regel verwijderen. In **Regels voor opname in de whitelist** wordt het gehele toepassingspad (met inbegrip van het uitvoerbare bestand) weergegeven voor de toepassingen die niet zijn ondertekend met een digitaal certificaat, maar nog steeds actueel zijn in de regels voor opname in de whitelist. 

> [!NOTE]
> Als best practice voor beveiliging probeert Security Center standaard altijd een uitgeverregel te maken voor de EXE’s die in de whitelist moeten worden opgenomen. Alleen als een EXE niet de gegevens van een uitgever bevat (ofwel niet is ondertekend), wordt er een padregel gemaakt voor het volledige pad van de specifieke EXE.

![Regels voor opname in de whitelist voor paden](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

De lijst bevat:
- **NAAM**: de volledige patch van het uitvoerbare bestand
- **EXPLOITEERBAAR**: 'waar' geeft aan dat toepassingen kunnen worden gebruikt door een aanvaller om opname in de whitelist van toepassingen te omzeilen.  

Als u op de drie punten aan het einde van elke regel klikt, kunt u die specifieke regel verwijderen. Nadat u de wijzigingen hebt aangebracht, kunt u klikken op de knop **Opslaan**. Als u besluit om de wijzigingen niet toe te passen, klikt u op de knop **Negeren**.

### <a name="not-recommended-list"></a>De lijst Niet aanbevolen

Security Center raadt alleen opname in de whitelist aan voor virtuele machines waarop een stabiele set toepassingen wordt uitgevoerd. Opname in de whitelist wordt niet aanbevolen als de toepassingen op de bijbehorende virtuele machines voortdurend wisselen. 

![Aanbeveling](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

De lijst bevat:
- **NAAM**: de naam van het abonnement en de resourcegroep.
- **VM’s**: het aantal virtuele machines in de resourcegroep.

## <a name="see-also"></a>Zie ook
In dit document hebt u geleerd hoe u gebruik kunt maken van adaptieve toepassingsbesturingselementen in Azure Security Center om toepassingen die op virtuele Azure-machines worden uitgevoerd, op te nemen in de whitelist. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center. 
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.


---
title: Adaptieve toepassingsbesturingselementen in Azure Security Center | Microsoft Docs
description: In dit document leert u meer over het gebruik van adaptieve toepassingsbesturingselementen in Azure Security Center om toepassingen die worden uitgevoerd op Azure VM's in de whitelist op te nemen.
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
ms.date: 09/14/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: 47ba7c7004ecf68f4a112ddf391eb645851ca1fb
ms.openlocfilehash: 18ae6a970455646b7a25170f5abefa52a98b0ba2
ms.contentlocale: nl-nl
ms.lasthandoff: 09/14/2017

---
# <a name="adaptive-application-controls-in-azure-security-center-preview"></a>Adaptieve toepassingsbesturingselementen in Azure Security Center (preview)
Lees hoe u toepassingsbeheer configureert in Azure Security Center met behulp van dit stapsgewijze overzicht.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Wat zijn adaptieve toepassingsbesturingselementen in Security Center?
Adaptieve toepassingsbesturingselementen helpen bepalen welke toepassingen kunnen worden uitgevoerd op uw virtuele machines die zich in Azure bevinden. Dit helpt u, naast andere voordelen, om uw virtuele machines beter bestand te maken tegen schadelijke software. Security Center maakt gebruik van machine learning voor het analyseren van de processen die op de virtuele machine worden uitgevoerd en helpt u met behulp van deze informatie bij het toepassen van regels voor opname in de whitelist. Deze mogelijkheid vereenvoudigt het proces van het configureren en onderhouden van whitelists aanzienlijk. U kunt hiermee het volgende:

- Pogingen schadelijke toepassingen uit te voeren, blokkeren of er een waarschuwing over verzenden, waaronder pogingen die mogelijk anders worden gemist door antimalwareoplossingen
- Voldoen aan het beveiligingsbeleid van uw organisatie die het gebruik van alleen gelicentieerde software bepaalt.
- Vermijden dat ongewenste software wordt gebruikt in uw omgeving.
- Vermijden dat oude en niet-ondersteunde apps worden uitgevoerd.
- Voorkomen van bepaalde softwareprogramma's die niet zijn toegestaan in uw organisatie.
- IT in staat stellen de toegang tot gevoelige gegevens te bepalen via het gebruik van apps.

## <a name="how-to-enable-adaptive-application-controls"></a>Hoe worden adaptieve toepassingsbesturingselementen ingeschakeld?
Adaptieve toepassingsbesturingselementen helpen u bij het definiëren van een set toepassingen die mogen worden uitgevoerd in de geconfigureerde resourcegroepen. Deze functie is alleen beschikbaar voor Windows-machines (alle versies, klassieke of Azure Resource Manager). Volg de onderstaande stappen voor het configureren van opname in de whitelist van toepassingen in Security Center:

1.  Open het **Security Center**-dashboard en klik op **Overzicht**.
2.  Onder **Geavanceerde cloudbeveiliging** toont de tegel **Adaptieve toepassingsbesturingselementen** voor hoeveel virtuele machines het besturingselement momenteel actief is in vergelijking met alle virtuele machines. Ook toont de tegel het aantal problemen dat de afgelopen week is gevonden: 

    ![Adaptieve toepassingsbesturingselementen](./media/security-center-adaptive-application\security-center-adaptive-application-fig1.png)

3. Klik op de tegel **Adaptieve toepassingsbesturingselementen** voor meer opties.

    ![besturingselementen](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

4. De sectie Resourcegroepen bevat drie tabbladen:
    * **Aanbevolen**: lijst met resourcegroepen waarvoor toepassingsbeheer wordt aanbevolen. Security Center maakt gebruik van machine learning voor het identificeren van virtuele machines die goede kandidaten zijn voor toepassingsbeheer afhankelijk van de vraag of de virtuele machines consequent dezelfde toepassingen uitvoeren.
    * **Geconfigureerd**: lijst met resourcegroepen met de virtuele machines die zijn geconfigureerd met toepassingsbeheer. 
    * **Geen aanbeveling**: lijst met resourcegroepen met virtuele machines zonder aanbevelingen voor toepassingsbeheer. Bijvoorbeeld virtuele machines waarop toepassingen steeds wisselen en geen stabiele status hebben.

In de volgende secties worden alle opties en het gebruik ervan gedetailleerder besproken.

### <a name="configure-a-new-application-control-policy"></a>Een nieuw toepassingsbeheerbeleid configureren
Klik op het tabblad **Aanbevolen** voor een lijst met resourcegroepen met aanbevelingen voor toepassingsbeheer:

![Aanbevolen](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

De lijst bevat:
- **NAAM**: de naam van het abonnement en de resourcegroep
- **VM’s**: het aantal virtuele machines in de resourcegroep
- **STATUS**: de status van de aanbevelingen, die in de meeste gevallen 'open' is
- **ERNST**: de ernst van de aanbevelingen

Selecteer een resourcegroep om de optie **maken van regels voor toepassing** te openen:

![Regels voor toepassingsbeheer](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

Bekijk in **VM's selecteren** de lijst met aanbevolen VM's en schakel alle VM’s waarop u geen toepassingsbeheer wilt toepassen uit. Bekijk in **Processen selecteren voor de regels voor opname in de whitelist** de lijst met aanbevolen toepassingen en schakel alle toepassingen waarop u geen toepassingsbeheer wilt toepassen uit. De lijst bevat:

- **NAAM**: het volledige toepassingspad
- **PROCESSEN**: hoeveel toepassingen zich binnen elke pad bevinden
- **ALGEMEEN**: 'waar' geeft aan dat deze processen zijn uitgevoerd op de meeste VM's in deze resourcegroep.
- **EXPLOITEERBAAR**: er wordt een waarschuwingspictogram weergegeven als de toepassingen door een aanvaller kunnen worden gebruikt om opname in de whitelist van toepassingen te omzeilen. Het is raadzaam deze toepassingen te controleren voordat u deze goedkeurt. 

Klik op de knop **Maken** als u uw selecties hebt gemaakt. Standaard schakelt Security Center toepassingsbeheer altijd in de modus *Controle* in. Na validatie dat de whitelist geen negatieve gevolgen heeft voor uw workload, kunt u overschakelen naar de modus *Afdwingen*.

> [!NOTE]
> Als een best practice voor beveiliging probeert Security Center standaard altijd een uitgeverregel te maken voor de toepassingen die in de whitelist moeten worden opgenomen, en alleen als een toepassing niet de gegevens van een uitgever heeft (ofwel niet is ondertekend), wordt een padregel gemaakt voor het volledige pad van de specifieke EXE.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Een groep die is geconfigureerd met toepassingsbeheer bewerken en bewaken

Om een groep die is geconfigureerd met toepassingsbeheer te bewerken en te bewaken, klikt u op **GECONFIGUREERD** onder **Resourcegroepen**:

![Resourcegroepen](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

De lijst bevat:

- **NAAM**: de naam van het abonnement en de resourcegroep
- **VM’s**: het aantal virtuele machines in de resourcegroep
- **MODUS**: de controlemodus legt pogingen tot het uitvoeren van niet in de whitelist opgenomen toepassingen vast; met Blokkeren wordt niet toegestaan dat niet in de whitelist opgenomen toepassingen worden uitgevoerd
- **ERNST**: de ernst van de aanbevelingen

Selecteer een resourcegroep om wijzigingen aan te brengen op de pagina **Toepassingsbeheerbeleid bewerken**.

![Beveiliging](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

Onder **Beveiligingsmodus** kunt u een keuze maken uit de volgende opties:
- **Controle**: in deze modus dwingt de oplossing voor toepassingsbeheer de regels niet af, en wordt alleen de activiteit op de beveiligde virtuele machines gecontroleerd. Dit wordt aanbevolen voor scenario's waar u eerst het algehele gedrag wilt observeren voordat de uitvoering van een app op de doel-VM wordt geblokkeerd.
- **Afdwingen**: in deze modus dwingt de oplossing voor toepassingsbeheer de regels af en zorgt deze ervoor dat toepassingen die niet mogen worden uitgevoerd, worden geblokkeerd. 

Zoals eerder vermeld wordt een nieuw toepassingsbeheerbeleid altijd standaard geconfigureerd in de modus *Controle*. Onder **Beleidsuitbreiding** kunt u uw eigen toepassingspaden toevoegen die u in de whitelist wilt opnemen. Als u deze paden hebt toegevoegd, maakt Security Center de juiste regels voor deze toepassingen, naast de regels die al aanwezig zijn. In de sectie **Problemen** worden eventuele overtredingen vermeld.

![Problemen](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

Deze lijst bevat:

- **Problemen**: eventuele overtredingen die zijn geregistreerd, waaronder de volgende:
    - **ViolationsBlocked**: wanneer de oplossing op de modus Afdwingen is ingesteld en er wordt geprobeerd een toepassing uit te voeren die niet in de whitelist is opgenomen.
    - **ViolationsAudited**: wanneer de oplossing in de modus Controle is ingesteld en een toepassing die niet in de whitelist is opgenomen wordt uitgevoerd.
    - **RulesViolatedManually**: wanneer een gebruiker heeft geprobeerd handmatig regels te configureren op de virtuele machines en niet via de ASC-beheerportal.
- **AANTAL VM’s**: het aantal virtuele machines met dit type probleem.

Als u op elk van deze regels klikt, wordt u omgeleid naar de pagina [Azure Activity Log](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), waar u informatie over alle VM's met dit type overtreding kunt zien. Als u op de drie punten aan het einde van elke regel klikt, kunt u die specifieke vermelding verwijderen. In de sectie **Geconfigureerde virtuele machines** worden de virtuele machines weergegeven waarop deze regels van toepassing zijn. 

![Geconfigureerde virtuele machines](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

In **Uitgeverregels voor opname in de whitelist** worden de toepassingen vermeld waarvoor een uitgeverregel is gemaakt op basis van de gegevens van het certificaat dat is gevonden voor elke toepassing. Zie [Uitgeverregels in Applocker](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker) voor meer informatie.

![Regels voor opname in de whitelist](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

Als u op de drie punten aan het einde van elke regel klikt, kunt u die specifieke regel verwijderen. In **Regels voor opname in de whitelist** wordt het gehele toepassingspad (met inbegrip van het uitvoerbare bestand) weergegeven voor de toepassingen die niet zijn ondertekend met een digitaal certificaat, maar nog steeds actueel zijn in de regels voor opname in de whitelist. 

> [!NOTE]
> Als een best practice voor beveiliging probeert Security Center altijd een uitgeverregel te maken voor de EXE’s die in de whitelist moeten worden opgenomen, en alleen als een EXE niet de gegevens van een uitgever heeft (ofwel niet is ondertekend), wordt een padregel gemaakt voor het volledige pad van de specifieke EXE.

![Regels voor opname in de whitelist voor paden](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

De lijst bevat:
- **NAAM**: de volledige patch van het uitvoerbare bestand
- **EXPLOITEERBAAR**: 'waar' geeft aan dat toepassingen kunnen worden gebruikt door een aanvaller om opname in de whitelist van toepassingen te omzeilen.  

Als u op de drie punten aan het einde van elke regel klikt, kunt u die specifieke regel verwijderen. Nadat u de wijzigingen hebt aangebracht, kunt u klikken op de knop **Opslaan**. Als u besluit om de wijzigingen niet toe te passen, kikt u op de knop **Negeren**.

### <a name="not-recommended-list"></a>De lijst Niet aanbevolen

Security Center raadt alleen opname in de whitelist aan voor virtuele machines waarop een stabiele set toepassingen wordt uitgevoerd. Er wordt geen aanbeveling gedaan als toepassingen op de bijbehorende virtuele machines voortdurend wisselen. 

![Aanbeveling](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

De lijst bevat:
- **NAAM**: de naam van het abonnement en de resourcegroep.
- **VM’s**: het aantal virtuele machines in de resourcegroep.

## <a name="preview-registration"></a>Registratie voor preview

Adaptieve toepassingsbesturingselementen zijn beschikbaar voor klanten met Azure Security Center Standard als een beperkte openbare preview. Stuur [ons](mailto:ASC_appcontrol@microsoft.com) een e-mail met uw abonnement-id's om deel te nemen aan de preview.

## <a name="see-also"></a>Zie ook
In dit document hebt u geleerd hoe u gebruik kunt maken van adaptieve toepassingsbesturingselementen in Azure Security Center om toepassingen die worden uitgevoerd op Azure VM's op te nemen in de whitelist. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center. 
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.



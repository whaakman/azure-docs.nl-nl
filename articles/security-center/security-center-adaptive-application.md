---
title: Adaptieve toepassingsbesturingselementen in Azure Security Center | Microsoft Docs
description: In dit document leert u hoe u adaptieve toepassingsbesturingselementen in Azure Security Center gebruikt om toepassingen die op virtuele Azure-machines worden uitgevoerd, in de whitelist op te nemen.
services: security-center
documentationcenter: na
author: rkarlin
manager: mbaldwin
editor: ''
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/31/2018
ms.author: rkarlin
ms.openlocfilehash: 6dd971ff8cd51435978ab80db006f6494dff8a94
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39389324"
---
# <a name="adaptive-application-controls-in-azure-security-center"></a>Adaptieve toepassingsbesturingselementen in Azure Security Center
Lees hoe u toepassingsbeheer configureert in Azure Security Center met behulp van dit stapsgewijze overzicht.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Wat zijn adaptieve toepassingsbesturingselementen in Security Center?
Adaptieve toepassingsbesturingselementen helpen bepalen welke toepassingen kunnen worden uitgevoerd op uw virtuele machines die zich in Azure bevinden. Dit biedt u diverse voordelen, waaronder de mogelijkheid om uw virtuele machines beter bestand te maken tegen schadelijke software. Security Center maakt gebruik van machine learning om de toepassingen te analyseren die op de virtuele machine worden uitgevoerd. Ook helpt het u op basis van deze informatie regels voor opname in de whitelist toe te passen. Met deze mogelijkheid wordt het configureren en onderhouden van whitelists aanzienlijk eenvoudiger. U kunt hiermee het volgende:

- Pogingen om schadelijke toepassingen uit te voeren, blokkeren of hier een waarschuwing over verzenden, waaronder pogingen die anders mogelijk worden gemist door antimalwareoplossingen.
- Voldoen aan het beveiligingsbeleid van uw organisatie waarin alleen het gebruik van gelicentieerde software is toegestaan.
- Vermijden dat in uw omgeving ongewenste software wordt gebruikt.
- Vermijden dat oude en niet-ondersteunde apps worden uitgevoerd.
- Voorkomen dat bepaalde softwareprogramma's worden uitgevoerd die in uw organisatie niet zijn toegestaan.
- De IT-afdeling in staat stellen de toegang tot gevoelige gegevens te beheren via het gebruik van apps.

## <a name="how-to-enable-adaptive-application-controls"></a>Hoe worden adaptieve toepassingsbesturingselementen ingeschakeld?
Met adaptieve toepassingsbesturingselementen kunt u een set toepassingen definiëren die in de geconfigureerde groepen mogen worden uitgevoerd. Deze functie is alleen beschikbaar voor Windows-machines (alle versies, klassieke implementatie of Azure Resource Manager-implementatie). Met de onderstaande stappen kunt u de whitelist met toepassingen in Security Center configureren:

1. Open het dashboard van **Security Center**.
2. Ga naar het deelvenster aan de linkerkant en selecteer **Adaptieve toepassingsbesturingselementen** onder **Geavanceerde cloudbeveiliging**.

    ![Defensie](./media/security-center-adaptive-application/security-center-adaptive-application-fig1-new.png)

De pagina **Besturingselementen voor adaptieve toepassingen** wordt geopend.

![besturingselementen](./media/security-center-adaptive-application/security-center-adaptive-application-fig2.png)

De sectie **Groepen virtuele machines** bevat drie tabbladen:

* **Geconfigureerd**: een lijst van groepen met de VM's waarvoor toepassingsbeheer is geconfigureerd.
* **Aanbevolen**: een lijst met groepen waarvoor toepassingsbeheer wordt aanbevolen. Security Center maakt gebruik van machine learning om virtuele machines te identifiveren die baat zouden hebben bij het gebruik van toepassingsbeheer. Hierbij wordt gekeken of de virtuele machines consequent dezelfde toepassingen uitvoeren.
* **Geen aanbeveling**: een lijst van groepen met VM's waarvoor geen aanbevelingen zijn gedaan voor het gebruik van toepassingsbeheer. Hierbij kan het bijvoorbeeld gaan om virtuele machines waarop toepassingen steeds wisselen en geen stabiele status hebben.

> [!NOTE]
> Security Center gebruikt een eigen clustering-algoritme voor groepen met VM's om ervoor te zorgen dat vergelijkbare VM’s het optimale aanbevolen toepassingsbeheerbeleid krijgen.
>
>

### <a name="configure-a-new-application-control-policy"></a>Een nieuw toepassingsbeheerbeleid configureren
1. Klik op het tabblad **Aanbevolen** voor een lijst met groepen waarvoor toepassingsbeheer wordt aanbevolen:

  ![Aanbevolen](./media/security-center-adaptive-application/security-center-adaptive-application-fig3.png)

  De lijst bevat:

  - **NAAM**: de naam van het abonnement en de groep
  - **VM's**: het aantal virtuele machines in de groep
  - **STATUS**: de status van de aanbevelingen (in de meeste gevallen 'open')
  - **ERNST**: de ernst van de aanbevelingen

2. Selecteer een resourcegroep om de optie **Regels voor toepassingsbeheer maken** te openen.

  ![Regels voor toepassingsbeheer](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

3. Bekijk in **VM's selecteren** de lijst met aanbevolen virtuele machines en schakel alle virtuele machines uit waarop u geen toepassingsbeheer wilt toepassen. Vervolgens ziet u twee lijsten:

  - **Aanbevolen toepassingen**: een lijst met toepassingen die vaak op de virtuele machines in deze groep voorkomen, en dus aanbevolen zijn voor controleregels voor toepassingen door Security Center.
  - **Meer toepassingen**: een lijst met toepassingen die minder frequent op de virtuele machines in deze groep voorkomen of die worden aangeduid als Exploitables (zie hieronder meer), en worden aanbevolen voor controle voordat de regels worden toegepast.

4. Bekijk de toepassingen in elk van de lijsten en schakel uit wat u niet wilt toepassen. Elke lijst bevat:

  - **NAAM**: de gegevens van een toepassing of het volledige pad van de toepassing
  - **BESTANDSTYPEN**: het bestandstype van de toepassing. Dit kan EXE, Script of MSI zijn.
  - **EXPLOITEERBAAR**: er wordt een waarschuwingspictogram weergegeven als de toepassingen door een aanvaller kunnen worden gebruikt om opname in de whitelist met toepassingen te omzeilen. U wordt aanbevolen om deze toepassingen te controleren voordat ze worden goedgekeurd.
  - **GEBRUIKERS**: gebruikers die worden aanbevolen om een toepassing uit te mogen voeren

5. Selecteer **Maken** nadat u uw selecties hebt gemaakt.


> [!NOTE]
> - Security Center heeft minimaal twee weken aan gegevens nodig om een basislijn te maken en de unieke aanbevelingen per groep virtuele machines te vullen. Voor nieuwe klanten van de Security Center-standaardcategorie worden groepen virtuele machines in het begin weergegeven op het tabblad *Geen aanbeveling*.
> - Adaptieve toepassingsbesturingselementen in Security Center bieden geen ondersteuning voor virtuele machines waarvoor al een AppLocker-beleid is ingeschakeld door een groepsbeleidsobject (GPO) of een lokaal beveiligingsbeleid.
> -  Als best practice voor beveiliging probeert Security Center standaard altijd een uitgeverregel te maken voor de toepassingen die in de whitelist moeten worden opgenomen. Alleen als een toepassing niet de gegevens van een uitgever bevat (ofwel niet is ondertekend), wordt er een padregel gemaakt voor het volledige pad van de specifieke EXE.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Een groep die met toepassingsbeheer is geconfigureerd, bewerken en bewaken

1. Als u een groep wilt bewerken en controleren die is geconfigureerd met toepassingsbeheer, gaat u terug naar de pagina **Adaptieve toepassingsbesturingselementen** en selecteert u **GECONFIGUREERD** onder **Groepen virtuele machines**:

  ![Groepen](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

  De lijst bevat:

  - **NAAM**: de naam van het abonnement en de groep
  - **VM's**: het aantal virtuele machines in de groep
  - **MODUS**: de controlemodus legt pogingen tot uitvoering van niet in de whitelist opgenomen toepassingen vast. Afdwingen zorgt ervoor dat niet in de whitelist opgenomen toepassingen niet worden uitgevoerd.
  - **PROBLEMEN**: eventuele actuele overtredingen

2. Selecteer een groep om wijzigingen in aan te brengen op de pagina **Beleid voor toepassingsbeheer bewerken**.

  ![Beveiliging](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

3. Onder **Beveiligingsmodus** kunt u een keuze maken uit de volgende opties:

  - **Controle**: in deze modus worden de regels niet afgedwongen door de oplossing voor toepassingsbeheer en wordt alleen de activiteit op de beveiligde virtuele machines gecontroleerd. Dit wordt aanbevolen voor scenario's waarin u eerst het algehele gedrag wilt observeren voordat de uitvoering van een app op de doel-VM wordt geblokkeerd.
  - **Afdwingen**: in deze modus worden de regels wel afgedwongen door de oplossing voor toepassingsbeheer. Ook worden toepassingen die niet mogen worden uitgevoerd, geblokkeerd.

  Zoals eerder vermeld, wordt een nieuw beleid voor toepassingsbeheer altijd standaard geconfigureerd in de modus *Controle*. Onder **Beleidsuitbreiding** kunt u uw eigen toepassingspaden toevoegen die u in de whitelist wilt opnemen. Nadat u deze paden hebt toegevoegd, maakt Security Center de juiste regels voor deze toepassingen, naast de regels die al aanwezig zijn.

  In de sectie **Recente problemen** worden eventuele actuele overtredingen vermeld.

  ![Problemen](./media/security-center-adaptive-application/security-center-adaptive-application-fig7.png)

  Deze lijst bevat:
  - **Problemen**: eventuele overtredingen die zijn geregistreerd, waaronder de volgende:

      - **ViolationsBlocked**: wanneer de oplossing in de modus Afdwingen is ingesteld en er wordt geprobeerd een toepassing uit te voeren die niet in de whitelist is opgenomen.
      - **ViolationsAudited**: wanneer de oplossing in de modus Controle is ingesteld en er een toepassing die niet in de whitelist is opgenomen, wordt uitgevoerd.

 - **AANTAL VM’s**: het aantal virtuele machines met dit type probleem.

  Als u op een regel klikt, wordt u doorgestuurd naar de pagina [Azure Activity Log](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs), waar u informatie over alle virtuele machines met dit type overtreding kunt zien. Als u op de drie punten aan het einde van elke regel klikt, kunt u die specifieke vermelding verwijderen. In de sectie **Geconfigureerde virtuele machines** worden de virtuele machines weergegeven waarop deze regels van toepassing zijn.

  ![Geconfigureerde virtuele machines](./media/security-center-adaptive-application/security-center-adaptive-application-fig8.png)

  De lijst onder **Uitgeverregels voor opname in de whitelist** bevat het volgende:

  - **REGEL**: toepassingen waarvoor een uitgeverregel is gemaakt op basis van de gegevens van het certificaat dat voor elke toepassing is gevonden
  - **BESTANDSTYPE**: de bestandstypen die worden gedekt door een specifieke uitgeversregel. Dit kan een van de volgende zijn: EXE, Script of MSI.
  - **GEBRUIKERS**: het aantal gebruikers dat elke toepassing kan uitvoeren

  Zie [Uitgeverregels in Applocker](https://docs.microsoft.com/windows/device-security/applocker/understanding-the-publisher-rule-condition-in-applocker) voor meer informatie.

  ![Regels voor opname in de whitelist](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

  Als u op de drie punten aan het einde van elke regel klikt, kunt u die specifieke regel verwijderen of de toegestane gebruikers bewerken.

  In de sectie **Regels voor opname van paden in de whitelist** wordt het gehele toepassingspad (met inbegrip van het uitvoerbare bestand) weergegeven voor de toepassingen die niet zijn ondertekend met een digitaal certificaat, maar nog steeds actueel zijn in de regels voor opname in de whitelist.

  > [!NOTE]
  > Als best practice voor beveiliging probeert Security Center standaard altijd een uitgeverregel te maken voor de EXE’s die in de whitelist moeten worden opgenomen. Alleen als een EXE niet de gegevens van een uitgever bevat (ofwel niet is ondertekend), wordt er een padregel gemaakt voor het volledige pad van de specifieke EXE.

  ![Regels voor opname in de whitelist voor paden](./media/security-center-adaptive-application/security-center-adaptive-application-fig10.png)

  De lijst bevat:
  - **NAAM**: de volledige patch van het uitvoerbare bestand
  - **BESTANDSTYPE**: de bestandstypen die worden gedekt door een specifieke padregel. Dit kan een van de volgende zijn: EXE, Script of MSI.
  - **GEBRUIKERS**: het aantal gebruikers dat elke toepassing kan uitvoeren

  Als u op de drie punten aan het einde van elke regel klikt, kunt u die specifieke regel verwijderen of de toegestane gebruikers bewerken.

4. Nadat u wijzigingen hebt aangebracht op de pagina **Adaptieve toepassingsbesturingselementen**, klikt u op de knop **Opslaan**. Als u de wijzigingen toch niet wilt toepassen, klikt u op **Verwijderen**.

### <a name="not-recommended-list"></a>De lijst Niet aanbevolen

Security Center raadt alleen opname in de whitelist aan voor virtuele machines waarop een stabiele set toepassingen wordt uitgevoerd. Opname in de whitelist wordt niet aanbevolen als de toepassingen op de bijbehorende virtuele machines voortdurend wisselen.

![Aanbeveling](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

De lijst bevat:
- **NAAM**: de naam van het abonnement en de groep
- **VM's**: het aantal virtuele machines in de groep

## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u gebruik kunt maken van adaptieve toepassingsbesturingselementen in Azure Security Center om toepassingen die op virtuele Azure-machines worden uitgevoerd, op te nemen in de whitelist. Zie de volgende onderwerpen voor meer informatie over het Azure Beveiligingscentrum:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.

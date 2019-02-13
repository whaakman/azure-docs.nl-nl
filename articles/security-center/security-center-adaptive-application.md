---
title: Adaptieve toepassingsbesturingselementen in Azure Security Center | Microsoft Docs
description: In dit document leert u hoe u adaptieve toepassingsbesturingselementen in Azure Security Center gebruikt om toepassingen die op virtuele Azure-machines worden uitgevoerd, in de whitelist op te nemen.
services: security-center
documentationcenter: na
author: monhaber
manager: barbkess
editor: ''
ms.assetid: 9268b8dd-a327-4e36-918e-0c0b711e99d2
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/21/2019
ms.author: monhaber
ms.openlocfilehash: d92d9c444985989cdec3e94840ff21f4a79fe3ad
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56104921"
---
# <a name="adaptive-application-controls-in-azure-security-center"></a>Adaptieve toepassingsbesturingselementen in Azure Security Center
Lees hoe u toepassingsbeheer configureert in Azure Security Center met behulp van dit stapsgewijze overzicht.

## <a name="what-are-adaptive-application-controls-in-security-center"></a>Wat zijn adaptieve toepassingsbesturingselementen in Security Center?
Adaptieve toepassingsbesturingselementen is een oplossing voor het in de whitelist van intelligente, geautomatiseerde end-to-end-toepassingen van Azure Security Center. Hiermee kunt u bepalen welke toepassingen kunnen worden uitgevoerd op uw virtuele machines die zich in Azure, die onder andere voordelen, helpt de beveiliging van uw virtuele machines tegen schadelijke software. Security Center maakt gebruik van machine learning voor het analyseren van de toepassingen die worden uitgevoerd op uw virtuele machines en helpt u bij het toepassen van de specifieke basis van deze informatie in de whitelist. Deze mogelijkheid vereenvoudigt het proces van het configureren en onderhouden van beleidsregels voor toepassingen in een whitelist opnemen, zodat u kunt:

- Blokkeren of waarschuwing op pogingen tot uitvoering van schadelijke toepassingen, waaronder pogingen die anders mogelijk worden gemist door antimalwareoplossingen.
- Voldoen aan het beveiligingsbeleid van uw organisatie waarin alleen het gebruik van gelicentieerde software is toegestaan.
- Vermijden dat in uw omgeving ongewenste software wordt gebruikt.
- Vermijden dat oude en niet-ondersteunde apps worden uitgevoerd.
- Voorkomen dat bepaalde softwareprogramma's worden uitgevoerd die in uw organisatie niet zijn toegestaan.
- De IT-afdeling in staat stellen de toegang tot gevoelige gegevens te beheren via het gebruik van apps.

## <a name="how-to-enable-adaptive-application-controls"></a>Hoe worden adaptieve toepassingsbesturingselementen ingeschakeld?
Besturingselementen voor adaptieve toepassingen helpen u bij het definiëren van een set toepassingen die mogen worden uitgevoerd op de geconfigureerde groepen virtuele machines. Deze functie is alleen beschikbaar voor Windows-machines (alle versies, klassieke implementatie of Azure Resource Manager-implementatie). Met de onderstaande stappen kunt u de whitelist met toepassingen in Security Center configureren:

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
  - **STATUS**: de status van de aanbevelingen
  - **ERNST**: de ernst van de aanbevelingen

2. Klik op een groep te openen de **regels voor toepassingsbeheer maken** optie.

  ![Regels voor toepassingsbeheer](./media/security-center-adaptive-application/security-center-adaptive-application-fig4.png)

3. In de **VM's selecteren**, Controleer de lijst met aanbevolen virtuele machines en schakel uit wat u niet wilt dat een toepassing in de whitelist aan beleid toepassen op. Vervolgens ziet u twee lijsten:

  - **Aanbevolen toepassingen**: een lijst met toepassingen die vaak op de virtuele machines in deze groep, en worden aanbevolen om mag worden uitgevoerd.
  - **Meer toepassingen**: een lijst met toepassingen die minder frequent op de virtuele machines in deze groep of die worden aangeduid als Exploitables (Zie hieronder meer), en aanbevolen voor controle.

4. Bekijk de toepassingen in elk van de lijsten en schakel uit wat u niet wilt toepassen. Elke lijst bevat:

  - **NAAM**: informatie over het certificaat of het volledige pad van een toepassing
  - **BESTANDSTYPEN**: het bestandstype van de toepassing. Dit kan EXE, Script, MSI-bestand of elk permutatie van de volgende typen zijn.
  - **EXPLOITEERBAAR**: een waarschuwingspictogram geeft aan dat als een bepaalde toepassing door een aanvaller kan worden gebruikt om een oplossing voor het in de whitelist aan over te slaan. U wordt aanbevolen om deze toepassingen te controleren voordat ze worden goedgekeurd.
  - **GEBRUIKERS**: gebruikers die worden aanbevolen om een toepassing uit te mogen voeren

5. Selecteer **Maken** nadat u uw selecties hebt gemaakt. <br>
Nadat u maken selecteert, maakt Azure Security Center automatisch de juiste regels boven op de ingebouwde toepassing opname in de whitelist oplossing die beschikbaar op Windows-servers (AppLocker).

> [!NOTE]
> - Security Center heeft minimaal twee weken aan gegevens nodig om een basislijn te maken en de unieke aanbevelingen per groep virtuele machines te vullen. Voor nieuwe klanten van de Security Center-standaardcategorie worden groepen virtuele machines in het begin weergegeven op het tabblad *Geen aanbeveling*.
> - Adaptieve toepassingsbesturingselementen in Security Center bieden geen ondersteuning voor virtuele machines waarvoor al een AppLocker-beleid is ingeschakeld door een groepsbeleidsobject (GPO) of een lokaal beveiligingsbeleid.
> -  Als een best practice Security Center wordt altijd geprobeerd om te maken van een uitgeversregel voor toepassingen die zijn geselecteerd worden toegestaan en alleen als een toepassing geen gegevens van een uitgever bevat (ofwel niet is ondertekend), wordt een padregel gemaakt voor het volledige pad van de specifieke toepassing.
>   

### <a name="editing-and-monitoring-a-group-configured-with-application-control"></a>Een groep die met toepassingsbeheer is geconfigureerd, bewerken en bewaken

1. Als u wilt bewerken en controleren van een groep die is geconfigureerd met een toepassing in de whitelist-beleid, gaat u terug naar de **besturingselementen voor adaptieve toepassingen** pagina en selecteer **geconfigureerd** onder **groepen virtuele machines**:

  ![Groepen](./media/security-center-adaptive-application/security-center-adaptive-application-fig5.png)

  De lijst bevat:

  - **Naam**: de naam van het abonnement en de groep
  - **VM's**: het aantal virtuele machines in de groep
  - **Modus**: Controlemodus legt pogingen tot uitvoering van niet-goedgekeurde toepassingen. Afdwingen van wordt niet toe dat niet-goedgekeurde toepassingen worden uitgevoerd
  - **Waarschuwingen**: eventuele actuele overtredingen

2. Klik op een groep te wijzigen in de **beleid voor toepassingsbeheer bewerken** pagina.

  ![Beveiliging](./media/security-center-adaptive-application/security-center-adaptive-application-fig6.png)

3. Onder **Beveiligingsmodus** kunt u een keuze maken uit de volgende opties:

  - **Controle**: in deze modus worden de regels niet afgedwongen door de oplossing voor toepassingsbeheer en wordt alleen de activiteit op de beveiligde virtuele machines gecontroleerd. Dit wordt aanbevolen voor scenario's waarin u eerst het algehele gedrag wilt observeren voordat de uitvoering van een app op de doel-VM wordt geblokkeerd.
  - **Afdwingen**: in deze modus worden de regels wel afgedwongen door de oplossing voor toepassingsbeheer. Ook worden toepassingen die niet mogen worden uitgevoerd, geblokkeerd.

   > [!NOTE]
   > -  **Afdwingen** beveiligingsmodus is uitgeschakeld tot nader order van kracht.
   > - Zoals eerder vermeld, wordt een nieuw beleid voor toepassingsbeheer altijd standaard geconfigureerd in de modus *Controle*. 
   >

4. Onder **beleidsuitbreiding**, voegt u een toepassingspad die u wilt toestaan. Nadat u deze paden hebt toegevoegd, kan Security Center werkt het beleid van de toepassing in de whitelist aan op de virtuele machines in de geselecteerde groep van virtuele machines en maakt de juiste regels voor deze toepassingen, naast de regels die al aanwezig.

5. Bekijk de actuele overtredingen vermeld in de **recente waarschuwingen** sectie. Klik op elke regel worden omgeleid naar de **waarschuwingen** pagina in Azure Security Center en alle waarschuwingen die zijn gedetecteerd door Azure Security Center op de bijbehorende virtuele machines weergeven.
  - **Waarschuwingen**: eventuele schendingen die zijn geregistreerd.
  - **Nee. van virtuele machines**: het aantal virtuele machines met dit type waarschuwing.

6. Onder **uitgeverregels**, **padregels voor opname in de whitelist**, en **hash-regels voor opname in de whitelist** ziet u welke opname in de whitelist regels momenteel zijn geconfigureerd op de virtuele machines in een groep, op basis van het type regelverzameling. Voor elke regel kunt u het volgende zien:

  - **Regel**: De specifieke parameters opgegeven op basis waarvan een toepassing wordt gecontroleerd door AppLocker om te bepalen of een toepassing is toegestaan om uit te voeren.
  - **Bestandstype**: De bestandstypen die worden gedekt door een specifieke regel. Dit kan een van de volgende zijn: EXE, Script, MSI-bestand of elk permutatie van deze bestandstypen.
  - **Gebruikers**: Naam of het aantal gebruikers die gemachtigd zijn om uit te voeren van een toepassing die wordt gedekt door een toepassing in de whitelist aan-regel.

   ![Regels voor opname in de whitelist](./media/security-center-adaptive-application/security-center-adaptive-application-fig9.png)

7. Klik op de drie puntjes aan het einde van elke regel als u wilt de specifieke regel verwijderen of de toegestane gebruikers bewerken.

8. Nadat u wijzigingen aanbrengen in een **besturingselementen voor adaptieve toepassingen** beleid, klikt u op **opslaan**.

### <a name="not-recommended-list"></a>De lijst Niet aanbevolen

Security Center raadt alleen opname in de whitelist toepassingsbeleid voor virtuele machines waarop een stabiele set toepassingen wordt uitgevoerd. Opname in de whitelist wordt niet aanbevolen als de toepassingen op de bijbehorende virtuele machines voortdurend wisselen.

![Aanbeveling](./media/security-center-adaptive-application/security-center-adaptive-application-fig11.png)

De lijst bevat:
- **NAAM**: de naam van het abonnement en de groep
- **VM's**: het aantal virtuele machines in de groep

Azure Security Center kunt u een toepassing in de whitelist om beleid te definiëren op niet-aanbevolen VM-groepen ook. Volg dezelfde principes als eerder zijn beschreven, een toepassing in de whitelist om beleid te configureren op deze groepen ook.


## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u gebruik kunt maken van adaptieve toepassingsbesturingselementen in Azure Security Center om toepassingen die op virtuele Azure-machines worden uitgevoerd, op te nemen in de whitelist. Zie de volgende onderwerpen voor meer informatie over het Azure Beveiligingscentrum:

* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Informatie over het beheren van waarschuwingen en het reageren op beveiligingsincidenten in Security Center.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Meer informatie over de verschillende typen beveiligingswaarschuwingen.
* [Handleiding voor het oplossen van problemen met Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Informatie over het oplossen van veelvoorkomende problemen met Security Center.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.

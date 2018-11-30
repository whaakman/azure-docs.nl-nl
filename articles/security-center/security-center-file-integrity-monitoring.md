---
title: Bestandsintegriteit controleren in Azure Security Center | Microsoft Docs
description: " Informatie over het inschakelen van de functie bestandsintegriteit controleren in Azure Security Center. "
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/21/2018
ms.author: rkarlin
ms.openlocfilehash: 9cfb098fd8e6cb777b6cd968f92b20baabfab705
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52313980"
---
# <a name="file-integrity-monitoring-in-azure-security-center"></a>Bestandsintegriteit controleren in Azure Security Center
Informatie over het configureren van bestand integriteit controleren (FIM) in Azure Security Center gebruikt deze stapsgewijze kennismaking.

## <a name="what-is-fim-in-security-center"></a>Wat is FIM in Security Center?
Bestand integriteit controleren (FIM), ook wel bekend als controle, onderzoekt bestanden en registers van het besturingssysteem, toepassingssoftware en andere wijzigingen die op een aanval wijzen kunnen. Een van de vergelijkingsmethode wordt gebruikt om te bepalen of de huidige status van het bestand af van de laatste scan van het bestand. U kunt gebruikmaken van deze vergelijking om te bepalen of er geldige of verdachte wijzigingen zijn aangebracht aan uw bestanden.

Security Center bestandsintegriteit controleren de integriteit van Windows-bestanden, het register van Windows en Linux-bestanden worden gevalideerd. U selecteert de bestanden die u wilt dat bewaakte doordat FIM. Security Center bewaakt bestanden met FIM ingeschakeld voor de activiteit, zoals:

- Bestand en registerlocaties maken en verwijderen
- Bestandswijzigingen (wijzigingen in de bestandsgrootte, access control lists en hash van de inhoud)
- Register wijzigingen (wijzigingen in grootte, toegang conrol lijsten, type en de inhoud)

Security Center raadt entiteiten te bewaken, die u kunt eenvoudig FIM inschakelen op. U kunt ook uw eigen beleid FIM of entiteiten voor het bewaken van definiëren. In deze procedure ziet u hoe.

> [!NOTE]
> De functie bestand integriteit controleren (FIM) werkt voor Windows en Linux-computers en VM's en is beschikbaar op de prijscategorie Standard van Security Center. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
FIM worden gegevens geüpload naar de Log Analytics-werkruimte. Gegevens in rekening gebracht, op basis van de hoeveelheid gegevens die u uploadt. Zie [Log Analytics-prijzen](https://azure.microsoft.com/pricing/details/log-analytics/) voor meer informatie.
>
>

> [!NOTE]
> FIM maakt gebruik van de oplossing wijzigingen bijhouden in Azure bij te houden en wijzigingen in uw omgeving identificeren. Wanneer de functie bestandsintegriteit controleren is ingeschakeld, hebt u een **bijhouden** resource van het type oplossing. Als u verwijdert de **bijhouden** resource dat u de integriteit van de functie in Security Center bewaken uitschakelen.
>
>

## <a name="which-files-should-i-monitor"></a>Welke bestanden moet ik bewaak?
U moet doen met de bestanden die essentieel zijn voor uw systeem en de toepassingen zijn bij het kiezen van welke bestanden om te controleren. Houd rekening met bestanden die u waarschijnlijk niet wijzigen zonder de planning te kiezen. Kiezen-bestanden die vaak worden gewijzigd door toepassingen of -besturingssysteem (zoals logboekbestanden en tekstbestanden) maken een groot aantal ruis waardoor het moeilijk om een aanval worden geïdentificeerd.

Security Center aangeraden welke bestanden u moeten controleren als een standaard op basis van bekende aanvalspatronen die bestand en registerlocaties wijzigingen bevatten.

## <a name="using-file-integrity-monitoring"></a>Bestandsintegriteit controleren met behulp van het bestand
1. Open het dashboard van **Security Center**.
2. In het linkerdeelvenster onder **geavanceerde Cloudbeveiliging**, selecteer **bestandsintegriteit controleren**.
![Security Center-dashboard][1]

**Bestandsintegriteit controleren** wordt geopend.
  ![Security Center-dashboard][2]

De volgende informatie is bedoeld voor elke werkruimte:

- Totaal aantal wijzigingen dat is opgetreden in de afgelopen week (ziet u mogelijk een streepje "-" als FIM is niet ingeschakeld op de werkruimte)
- Totaal aantal computers en virtuele machines rapporteren aan de werkruimte
- Geografische locatie van de werkruimte
- De werkruimte onder de valt Azure-abonnement

De volgende knoppen kan ook worden weergegeven voor een werkruimte:

- ![Pictogram inschakelen][3] Geeft aan dat FIM niet is ingeschakeld voor de werkruimte. De werkruimte te selecteren, kunt u FIM inschakelen op alle computers in de werkruimte.
- ![Upgrade-abonnement pictogram][4] geeft aan dat in de werkruimte of abonnement wordt niet uitgevoerd onder van Security Center Standard-laag. Voor het gebruik van de FIM-functie moet standaard op uw abonnement worden uitgevoerd.  De werkruimte te selecteren, kunt u een upgrade uitvoert naar Standard. Zie voor meer informatie over de Standard-laag en hoe u een upgrade uitvoert, [een upgrade uitvoert naar Standard van Security Center-prijscategorie voor verbeterde beveiliging](security-center-pricing.md).
- Een lege waarde (Er is geen knop) betekent dat FIM al is ingeschakeld op de werkruimte.

Onder **bestandsintegriteit controleren**, kunt u een werkruimte om het dashboard voor bestandsintegriteit controleren voor die werkruimte in te schakelen van FIM voor die werkruimte of [upgrade](security-center-pricing.md) de Standard-werkruimte.

## <a name="enable-fim"></a>FIM inschakelen
Om in te schakelen FIM in een werkruimte:

1. Onder **bestandsintegriteit controleren**, selecteer een werkruimte met de **inschakelen** knop.
2. **Bestandsintegriteit controleren inschakelen** Hiermee opent u het aantal Windows- en Linux-machines in de werkruimte weergegeven.

   ![Bestandsintegriteit controleren inschakelen][5]

   De aanbevolen instellingen voor Windows en Linux worden ook weergegeven.  Vouw **Windows bestanden**, **register**, en **Linux-bestanden** om te zien van de volledige lijst met aanbevolen items.

3. Schakel de aanbevolen entiteiten die u niet wilt toepassen FIM aan.
4. Selecteer **toepassen bestandsintegriteit controleren** om in te schakelen van FIM.

> [!NOTE]
> U kunt de instellingen op elk gewenst moment wijzigen. Zie [bewerken gecontroleerde entiteiten](security-center-file-integrity-monitoring.md#edit-monitored-items) hieronder voor meer informatie.
>
>

## <a name="view-the-fim-dashboard"></a>De FIM-dashboard weergeven
De **bestandsintegriteit controleren** dashboard wordt weergegeven voor werkruimten waarin FIM is ingeschakeld. De FIM-dashboard wordt geopend nadat u FIM in een werkruimte of wanneer u een werkruimte in de **bestandsintegriteit controleren** venster die al FIM ingeschakeld heeft.

![Bestand-dashboard voor bestandsintegriteit controleren][6]

De FIM-dashboard voor een werkruimte weergegeven het volgende:

- Totale aantal machines verbonden met de werkruimte
- Totaal aantal wijzigingen dat is opgetreden tijdens de geselecteerde periode
- Een overzicht van het wijzigingstype (bestanden, register)
- Een overzicht van de wijzigingscategorie (gewijzigd, toegevoegd, verwijderd)

Filter aan de bovenkant van het dashboard te selecteren, kunt u de periode die u wilt zien van de wijzigingen toepassen.

![Periode tijdfilter][7]

De **Computers** tabblad (hierboven) geeft een lijst van alle computers die rapporteren aan deze werkruimte. Voor elke machine, het dashboard bevat:

- Totaal aantal wijzigingen dat is opgetreden tijdens de geselecteerde periode
- Een overzicht van de totale wijzigingen als bestandswijzigingen of wijzigingen in het register

**Zoeken in logboeken** wordt geopend wanneer u een computernaam in het zoekvak invoert, veld of Selecteer een machine die wordt vermeld in het tabblad Computers. Zoeken in Logboeken worden alle wijzigingen in de geselecteerde periode voor de machine weergegeven. U kunt een wijziging voor meer informatie kunt uitbreiden.

![Zoeken in logboeken][8]

De **wijzigingen** tabblad (Zie hieronder) bevat alle wijzigingen in de werkruimte in de geselecteerde periode. Voor elke entiteit die is gewijzigd, het dashboard een lijst met de:

- Computer waarop de wijziging is opgetreden op
- Type wijziging (register of bestand)
- Soort wijziging (gewijzigd, toegevoegd, verwijderd)
- Datum en tijd van wijziging

![Wijzigingen voor de werkruimte][9]

**Details wijzigen** wordt geopend wanneer u een wijziging in het zoekvak invoert veld of Selecteer een entiteit die worden vermeld onder de **wijzigingen** tabblad.

![Details wijzigen][10]

## <a name="edit-monitored-entities"></a>Entiteiten bewerken die worden bewaakt

1. Ga terug naar de **dashboard voor bestandsintegriteit controleren** en selecteer **instellingen**.

  ![Instellingen][11]

  **Configuratie van de werkruimte** wordt geopend drie tabbladen: **Windows-register**, **Windows bestanden**, en **Linux-bestanden**. Elk tabblad geeft een lijst van de entiteiten die u in die categorie kunt bewerken. Voor elke entiteit die worden vermeld, Security Center wordt geïdentificeerd als FIM is ingeschakeld (true) of niet (ONWAAR) is ingeschakeld.  De entiteit bewerken, kunt u in- of uitschakelen van FIM.

  ![Configuratie van de werkruimte][12]

2. Selecteer een identityprotection. In dit voorbeeld wordt een item hebt geselecteerd onder Windows-register. **Voor het bijhouden van wijzigingen bewerken** wordt geopend.

  ![Bewerken of wijzigingen bijhouden][13]

Onder **voor het bijhouden van wijzigingen bewerken** kunt:

- (Waar) in- of uitschakelen (False) bestandsintegriteit controleren
- Opgeven of wijzig de naam van de entiteit
- Opgeven of wijzig de waarde of het pad
- De entiteit niet verwijderen, de wijziging negeren of sla de wijziging

## <a name="add-a-new-entity-to-monitor"></a>Een nieuwe entiteit voor het bewaken van toevoegen
1. Ga terug naar de **bestand integriteit bewaking dashboard** en selecteer **instellingen** aan de bovenkant. **Configuratie van de werkruimte** wordt geopend.
2. Onder **Werkruimteconfiguratie**, selecteer het tabblad voor het type entiteit die u wilt toevoegen: Windows-register, bestanden van Windows of Linux-bestanden. In dit voorbeeld wordt geselecteerd **Linux-bestanden**.

  ![Toevoegen van een nieuw item controleren][14]

3. Selecteer **Toevoegen**. **Toevoegen voor wijzigingen bijhouden** wordt geopend.

  ![Voer de gevraagde informatie][15]

4. Op de **toevoegen** pagina, typt u de gevraagde informatie op en selecteer **opslaan**.

## <a name="disable-monitored-entities"></a>Schakel bewaakt entiteiten
1. Ga terug naar de **bestandsintegriteit controleren** dashboard.
2. Selecteer een werkruimte waar FIM is momenteel ingeschakeld. Een werkruimte is ingeschakeld voor FIM als er de knop inschakelen of Upgrade abonnement ontbreekt.

  ![Selecteer een werkruimte waar FIM is ingeschakeld][16]

3. Selecteer onder de functie bestandsintegriteit controleren, **instellingen**.

  ![instellingen selecteren][17]

4. Onder **Werkruimteconfiguratie**, selecteert u een groep waarin **ingeschakeld** is ingesteld op true.

  ![Werkruimteconfiguratie][18]

5. Onder **voor het bijhouden van wijzigingen bewerken** venster set **ingeschakeld** op False.

  ![Ingeschakeld ingesteld op false][19]

6. Selecteer **Opslaan**.

## <a name="folder-and-path-monitoring-using-wildcards"></a>Map- en pad bewaking met jokertekens

Jokertekens gebruiken voor het vereenvoudigen van bijhouden in mappen. De volgende regels van toepassing wanneer u de map bewaking met jokertekens configureren:
-   Jokertekens zijn vereist voor het bijhouden van meerdere bestanden.
-   Jokertekens kunnen alleen worden gebruikt in het laatste segment van een pad, zoals C:\folder\file of /etc/*.conf
-   Als een omgevingsvariabele een pad dat is niet geldig bevat, validatie slaagt maar het pad mislukken wanneer inventarisatie wordt uitgevoerd.
-   Bij het instellen van het pad te voorkomen dat algemene paden, zoals c:\*. * zal dit leiden tot te veel mappen wordt gebruikt.

## <a name="disable-fim"></a>FIM uitschakelen
U kunt FIM uitschakelen. FIM maakt gebruik van de oplossing wijzigingen bijhouden in Azure bij te houden en wijzigingen in uw omgeving identificeren. FIM uitschakelt, kunt u de oplossing wijzigingen bijhouden verwijderen uit de geselecteerde werkruimte.

1. Als u wilt uitschakelen FIM, gaat u terug naar de **bestandsintegriteit controleren** dashboard.
2. Selecteer een werkruimte.
3. Onder **bestandsintegriteit controleren**, selecteer **uitschakelen**.

  ![FIM uitschakelen][20]

4. Selecteer **verwijderen** om uit te schakelen.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd bestand integriteit controleren (FIM) in Security Center gebruikt. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen](security-center-azure-policy.md) --informatie over het configureren van beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen.
* [Aanbevelingen voor beveiliging beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken](security-center-monitoring.md)--informatie over het bewaken van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md)--informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen bewaken](security-center-partner-solutions.md) --informatie over het bewaken van de status van uw partneroplossingen.
* [Security Center FAQ](security-center-faq.md): Raadpleeg Veelgestelde vragen over het gebruik van de service.
* [Azure Security Blog](https://blogs.msdn.com/b/azuresecurity/) (Azure-beveiligingsblog): hier vindt u het laatste nieuws over Azure-beveiliging en andere informatie.

<!--Image references-->
[1]: ./media/security-center-file-integrity-monitoring/security-center-dashboard.png
[2]: ./media/security-center-file-integrity-monitoring/file-integrity-monitoring.png
[3]: ./media/security-center-file-integrity-monitoring/enable.png
[4]: ./media/security-center-file-integrity-monitoring/upgrade-plan.png
[5]: ./media/security-center-file-integrity-monitoring/enable-fim.png
[6]: ./media/security-center-file-integrity-monitoring/fim-dashboard.png
[7]: ./media/security-center-file-integrity-monitoring/filter.png
[8]: ./media/security-center-file-integrity-monitoring/log-search.png
[9]: ./media/security-center-file-integrity-monitoring/changes-tab.png
[10]: ./media/security-center-file-integrity-monitoring/change-details.png
[11]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings.png
[12]: ./media/security-center-file-integrity-monitoring/workspace-config.png
[13]: ./media/security-center-file-integrity-monitoring/edit.png
[14]: ./media/security-center-file-integrity-monitoring/add.png
[15]: ./media/security-center-file-integrity-monitoring/add-item.png
[16]: ./media/security-center-file-integrity-monitoring/fim-dashboard-disable.png
[17]: ./media/security-center-file-integrity-monitoring/fim-dashboard-settings-disabled.png
[18]: ./media/security-center-file-integrity-monitoring/workspace-config-disable.png
[19]: ./media/security-center-file-integrity-monitoring/edit-disable.png
[20]: ./media/security-center-file-integrity-monitoring/disable-fim.png

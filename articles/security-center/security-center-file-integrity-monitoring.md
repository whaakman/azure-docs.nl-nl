---
title: Bestand integriteit bewaken in Azure Security Center (Preview) | Microsoft Docs
description: " Informatie over het inschakelen van bestand integriteit bewaken in Azure Security Center. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2018
ms.author: terrylan
ms.openlocfilehash: 722a4fd11f35f04ed22d73638f07d15c49ea3c26
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="file-integrity-monitoring-in-azure-security-center-preview"></a>Bestand integriteit bewaken in Azure Security Center (Preview)
Informatie over het configureren van bestand integriteit Monitoring (FIM) in Azure Security Center gebruikt deze stapsgewijze kennismaking.

## <a name="what-is-fim-in-security-center"></a>Wat is FIM in Security Center?
Bestand integriteit Monitoring (FIM), ook wel bekend als de wijziging bewaking, onderzoekt de bestanden en registers van besturingssystemen, softwaretoepassingen en anderen de wijzigingen die kunnen wijzen op een aanval. Een vergelijkingsmethode wordt gebruikt om te bepalen of de huidige status van het bestand anders dan de laatste scan van het bestand is. U kunt gebruikmaken van deze vergelijking om te bepalen als geldige of verdachte wijzigingen zijn aangebracht aan uw bestanden.

Security Center van integriteit bestandscontrole valideert de integriteit van de Windows-bestanden, Windows-register en Linux-bestanden. U selecteert de gewenste bestanden bewaakte doordat FIM. Security Center controleert bestanden met FIM ingeschakeld voor de activiteit, zoals:

- Bestands- en registermachtigingen maken en verwijderen
- Bestandswijzigingen (wijzigingen in de bestandsgrootte en toegangsbeheerlijsten hash van de inhoud)
- Register wijzigingen (wijzigingen in grootte, toegang conrol lijsten, type en de inhoud)

Security Center raadt entiteiten te bewaken, die u kunt eenvoudig FIM inschakelen op. U kunt ook uw eigen beleid FIM of entiteiten voor het bewaken van definiëren. Dit overzicht toont u hoe.

> [!NOTE]
> De functie bestand integriteit Monitoring (FIM) werkt voor Windows en Linux-computers en virtuele machines en is beschikbaar op de prijscategorie Standard van Security Center. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
FIM gegevens naar de werkruimte voor logboekanalyse geüpload. Kosten van de gegevens van toepassing, op basis van de hoeveelheid gegevens die u uploadt. Zie [logboekanalyse prijzen](https://azure.microsoft.com/pricing/details/log-analytics/) voor meer informatie.
>
>

> [!NOTE]
> FIM maakt gebruik van de oplossing voor het bijhouden van Azure voor het bijhouden en identificeren van wijzigingen in uw omgeving. Wanneer de integriteit bestandscontrole is ingeschakeld, hebt u een **bijhouden** bron van het type oplossing. Als u verwijdert de **bijhouden** resource, dat u de integriteit bewaken van de functie in Security Center uitschakelen.
>
>

## <a name="which-files-should-i-monitor"></a>Welke bestanden moet ik controleren?
U moet Denk na over de bestanden die essentieel voor uw systeem en toepassingen zijn bij het kiezen van welke bestanden om te controleren. U kunt bestanden die u niet verwacht dat wijzigen zonder een planning te kiezen. Kiezen bestanden die vaak worden gewijzigd door toepassingen of het besturingssysteem (zoals logboekbestanden en tekstbestanden) maken veel ruis waarmee het moeilijker een aanval worden geïdentificeerd.

Security Center raadt die bestanden u moeten controleren standaard volgens bekende aanvalspatronen die bestands- en registermachtigingen wijzigingen bevatten.

## <a name="using-file-integrity-monitoring"></a>Integriteit bewaken met behulp van het bestand
1. Open het dashboard van **Security Center**.
2. Klik in het linkerdeelvenster onder **geavanceerde Cloud verdediging**, selecteer **integriteit bestandscontrole**.
![Security Center-dashboard][1]

**Bestand integriteit bewaking** wordt geopend.
  ![Security Center-dashboard][2]

De volgende informatie is opgegeven voor elke werkruimte:

- Totaal aantal wijzigingen dat is opgetreden in de afgelopen week (mogelijk ziet u een streepje '-' als FIM is niet ingeschakeld op de werkruimte)
- Totaal aantal computers en virtuele machines in de werkruimte rapportage
- Geografische locatie van de werkruimte
- De werkruimte onder de ligt Azure-abonnement

De volgende knoppen kan ook worden weergegeven voor een werkruimte:

- ![Pictogram inschakelen][3] Hiermee wordt aangegeven of FIM niet is ingeschakeld voor de werkruimte. Als u de werkruimte selecteert, stelt u FIM op alle machines onder de werkruimte.
- ![Upgrade-abonnement pictogram][4] geeft aan dat in de werkruimte of het abonnement niet wordt uitgevoerd in Security Center de Standard-laag. Als u wilt de FIM-functie gebruikt, moet uw abonnement Standard worden uitgevoerd.  De werkruimte kunt u upgraden naar Standard. Zie voor meer informatie over de lagen Standard en bijwerken, [upgraden naar standaardcategorie voor verbeterde beveiliging van Security Center](security-center-pricing.md).
- Een leeg (Er is geen knop) betekent dat FIM al is ingeschakeld op de werkruimte.

Onder **integriteit bestandscontrole**, kunt u een werkruimte FIM inschakelen voor die werkruimte, het bestand integriteit bewakingsdashboard voor die werkruimte weergeven of [upgrade](security-center-pricing.md) de werkruimte op standaard.

## <a name="enable-fim"></a>FIM inschakelen
FIM in een werkruimte inschakelen:

1. Onder **integriteit bestandscontrole**, selecteer een werkruimte met de **inschakelen** knop.
2. **Schakel bewaking in bestand integriteit** Hiermee opent u het aantal Windows en Linux-machines onder de werkruimte weergeven.

   ![Bestand integriteit bewaking inschakelen][5]

   De aanbevolen instellingen voor Windows en Linux worden ook vermeld.  Vouw **Windows-bestanden**, **register**, en **Linux bestanden** om te zien van de volledige lijst met aanbevolen items.

3. Hiermee schakelt u de aanbevolen entiteiten die u niet wilt toepassen van FIM aan.
4. Selecteer **toepassen integriteit bestandscontrole** FIM inschakelen.

> [!NOTE]
> U kunt de instellingen op elk gewenst moment wijzigen. Zie [bewerken gecontroleerde entiteiten](security-center-file-integrity-monitoring.md#edit-monitored-items) hieronder voor meer informatie.
>
>

## <a name="view-the-fim-dashboard"></a>De FIM-dashboard weergeven
De **integriteit bestandscontrole** dashboard worden weergegeven voor werkruimten waarop FIM is ingeschakeld. De FIM-dashboard wordt geopend nadat het inschakelen van FIM in een werkruimte of wanneer u selecteert een werkruimte in de **integriteit bestandscontrole** venster dat al FIM ingeschakeld is.

![Bestand integriteit bewakingsdashboard][6]

De FIM-dashboard voor een werkruimte wordt het volgende weergegeven:

- Totale aantal machines verbonden met de werkruimte
- Totaal aantal wijzigingen dat is opgetreden tijdens de geselecteerde tijdsperiode
- Een uitsplitsing van wijzigingstype (bestanden, register)
- Een overzicht van de wijzigingscategorie (gewijzigd, toegevoegd, verwijderd)

Filter aan de bovenkant van het dashboard te selecteren, kunt u de periode die u wilt zien van wijzigingen voor toepassen.

![Tijdfilter periode][7]

De **Computers** tabblad (hierboven) een lijst met alle computers die rapporteren aan deze werkruimte. Voor elke computer, het dashboard bevat:

- Totaal aantal wijzigingen dat is opgetreden tijdens de geselecteerde periode
- Een overzicht van de totale wijzigingen als bestandswijzigingen of wijzigingen in het register

**Meld u zoekopdracht** wordt geopend wanneer u een computernaam in de zoekopdracht invoeren veld of Selecteer een machine die wordt vermeld in het tabblad Computers. Logboek zoeken geeft alle wijzigingen die tijdens de geselecteerde periode voor de machine. U kunt een wijziging voor meer informatie kunt uitbreiden.

![Zoeken in logboeken][8]

De **wijzigingen** tabblad (Zie hieronder) worden alle wijzigingen in de werkruimte tijdens de geselecteerde periode. Voor elke entiteit die is gewijzigd in de lijsten dashboard de:

- Computer die de wijziging is opgetreden op
- Soort wijziging (register of het bestand)
- Soort wijziging (gewijzigd, toegevoegd, verwijderd)
- Datum en tijd van wijziging

![Wijzigingen in de werkruimte][9]

**Details wijzigen** wordt geopend wanneer u een wijziging in de zoekopdracht veld of Selecteer een entiteit die worden vermeld onder de **wijzigingen** tabblad.

![Details wijzigen][10]

## <a name="edit-monitored-entities"></a>Entiteiten bewaakt bewerken

1. Ga terug naar de **bestand integriteit bewakingsdashboard** en selecteer **instellingen**.

  ![Instellingen][11]

  **Configuratie van de werkruimte** Hiermee opent u het weergeven van de drie tabbladen: **Windows-register**, **Windows-bestanden**, en **Linux bestanden**. Elk tabblad bevat de entiteiten die u in die categorie kunt bewerken. Voor elke entiteit die worden vermeld, Beveiligingscentrum identificeert als FIM is ingeschakeld (true) of niet (false) is ingeschakeld.  De entiteit bewerken, kunt u in- of uitschakelen van FIM.

  ![Configuratie van de werkruimte][12]

2. Selecteer een identityprotection. In dit voorbeeld wordt een item hebt geselecteerd onder Windows-register. **Voor het bijhouden bewerken** wordt geopend.

  ![Bewerken of het bijhouden van wijzigingen][13]

Onder **bewerken voor wijzigingen bijhouden** kunt u:

- (True) of schakel (False) bestand integriteit bewaking
- Opgeven of wijzig de entiteitsnaam
- Opgeven of wijzig de waarde of het pad
- De entiteit verwijderen, de wijziging negeren of sla de wijziging

## <a name="add-a-new-entity-to-monitor"></a>Voeg een nieuwe entiteit bewaken
1. Ga terug naar de **bestand integirty bewaking dashboard** en selecteer **instellingen** aan de bovenkant. **Configuratie van de werkruimte** wordt geopend.
2. Onder **configuratie van de werkruimte**, selecteer het tabblad voor het type entiteit die u wilt toevoegen: Windows-register, bestanden van Windows of Linux-bestanden. In dit voorbeeld wordt geselecteerd **Linux bestanden**.

  ![Een nieuw item voor het bewaken van toevoegen][14]

3. Selecteer **Toevoegen**. **Toevoegen voor het bijhouden** wordt geopend.

  ![Voer de gevraagde informatie][15]

4. Op de **toevoegen** pagina, typ de gevraagde informatie en selecteer **opslaan**.

## <a name="disable-monitored-entities"></a>Schakel bewaakt entiteiten
1. Ga terug naar de **integriteit bestandscontrole** dashboard.
2. Selecteer een werkruimte waar FIM is momenteel ingeschakeld. Een werkruimte is ingeschakeld voor FIM als er de knop inschakelen of upgraden van plan bent ontbreekt.

  ![Selecteer een werkruimte waarop FIM is ingeschakeld][16]

3. Selecteer onder integriteit bestandscontrole, **instellingen**.

  ![instellingen selecteren][17]

4. Onder **configuratie van de werkruimte**, selecteert u een groep waarbij **ingeschakeld** is ingesteld op true.

  ![Werkruimteconfiguratie][18]

5. Onder **bewerken voor wijzigingen bijhouden** venster set **ingeschakeld** op False.

  ![Ingeschakeld ingesteld op false][19]

6. Selecteer **Opslaan**.

## <a name="disable-fim"></a>FIM uitschakelen
U kunt FIM uitschakelen. FIM maakt gebruik van de oplossing voor het bijhouden van Azure voor het bijhouden en identificeren van wijzigingen in uw omgeving. FIM uitschakelen, verwijdert u de oplossing voor het bijhouden van de geselecteerde werkruimte.

1. Als u wilt uitschakelen FIM, terug naar de **integriteit bestandscontrole** dashboard.
2. Selecteer een werkruimte.
3. Onder **integriteit bestandscontrole**, selecteer **uitschakelen**.

  ![FIM uitschakelen][20]

4. Selecteer **verwijderen** om uit te schakelen.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd bestand integriteit Monitoring (FIM) in Security Center gebruiken. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Beveiligingsbeleid instellen](security-center-policies.md) --informatie over het beveiligingsbeleid voor uw Azure-abonnementen en resourcegroepen configureren.
* [Aanbevelingen voor beveiliging beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken](security-center-monitoring.md)--informatie over het bewaken van de status van uw Azure-resources.
* [Beheren en erop reageren op beveiligingswaarschuwingen](security-center-managing-and-responding-alerts.md)--informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen bewaken](security-center-partner-solutions.md) --informatie over het bewaken van de status van uw partneroplossingen.
* [Security Center FAQ](security-center-faq.md)--Raadpleeg Veelgestelde vragen over het gebruik van de service.
* [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Azure-beveiligingsblog): hier vindt u het laatste nieuws over Azure-beveiliging en andere informatie.

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

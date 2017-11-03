---
title: Voorbereiding voor Azure Security Center Standard voor een betere beveiliging | Microsoft Docs
description: " Meer informatie over hoe voor Onboarding van Azure Security Center Standard voor verbeterde beveiliging. "
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 411d7bae-c9d4-4e83-be63-9f2f2312b075
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: terrylan
ms.openlocfilehash: a6394b1b02ebfe518dc2f2b7f65eb677bb0ba5f2
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="onboarding-to-azure-security-center-standard-for-enhanced-security"></a>Voorbereiding voor Azure Security Center Standard voor een betere beveiliging
Upgrade uitvoeren naar Security Center Standard om te profiteren van betere beveiligingsbeheer en de bedreiging beveiliging voor de werkbelasting van uw hybride cloud.  U kunt standaard 60 dagen gratis uitproberen. Zie het Beveiligingscentrum [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.

Security Center-standaard bevat:

- **Hybride beveiliging** – een uniform overzicht krijgen van de beveiliging voor alle uw on-premises en in de cloud werkbelastingen. Toepassen van beveiligingsbeleid en de beveiliging van uw hybride cloud-workloads in overeenstemming met beveiligingsstandaarden continu beoordelen. Verzamel, zoek en analyseer beveiligingsgegevens van verschillende bronnen, inclusief firewalls en andere oplossingen van partners.
- **Geavanceerde detectie van dreigingen** -geavanceerde analyses en het Intelligent beveiliging voor Microsoft Graph een edge ophalen via de zich ontwikkelende cyberbeveiliging aanvallen gebruiken.  Maak gebruik van de ingebouwde gedragsanalyses en machine learning om aanvallen en zero-day exploits te identificeren. Controleer netwerken, computers en cloudservices op inkomende aanvallen en activiteiten na het schenden van de beveiliging. Stroomlijn het onderzoek met interactieve hulpprogramma's en contextuele bedreigingsinformatie.
- **Toegangs- en -besturingselementen** -blok malware en andere ongewenste toepassingen door het toepassen van aanbevelingen whitelisting aangepast aan uw specifieke werkbelastingen en aangedreven door machine learning. Beperk uw netwerk kwetsbaarheid met just-in-time, beheerde toegang tot beheerpoorten op Azure Virtual machines, drastisch te verminderen blootstelling aan brute force en andere netwerkaanvallen.

## <a name="detecting-unprotected-resources"></a>Niet-beveiligde bronnen detecteren     
Security Center detecteert automatisch een Azure-abonnementen of niet ingeschakeld voor Security Center Standard werkruimten. Dit omvat de Azure-abonnementen met Security Center gratis en werkruimten die geen van de oplossing beveiliging is ingeschakeld.

U kunt een volledige Azure-abonnement upgraden naar de prijscategorie Standard, die wordt overgenomen door alle resources binnen het abonnement, of u kunt een uniek beleid om bij te werken alleen een specifieke resourcegroep definiëren. Als de resourcegroep beleidsinstellingen uniek zijn, overschreven Security Center niet door de prijscategorie beleidsregels wanneer u het abonnement een naar de prijscategorie Standard upgrade. Toepassen van de standaard laag naar een abonnement is alleen van toepassing op de virtuele machines in het abonnement die aan werkruimten die zijn gemaakt door Security Center rapporteren. Toepassen van de standaard laag naar de werkruimte is van toepassing op alle resources in de werkruimte rapportage.

> [!NOTE]
> Mogelijk wilt de kosten beheren en beperken van de hoeveelheid gegevens die door het beperken van het aan een bepaalde set van agents naar een oplossing worden verzameld. [Oplossing targeting](../operations-management-suite/operations-management-suite-solution-targeting.md) kunt u een bereik van toepassing op de oplossing en gericht op een subset van computers in de werkruimte.  Als u van oplossing als doel gebruikmaakt, worden Security Center de werkruimte niet dat een oplossing.
>
>

## <a name="upgrade-an-azure-subscription"></a>Upgrade van een Azure-abonnement
Alle abonnementen op standaard upgraden:
1. Selecteer onder het hoofdmenu Security Center **Onboarding**.
2. Onder **voorbereiding voor geavanceerde beveiliging**, Security Center geeft een lijst van de abonnementen die in aanmerking voor onboarding. U kunt alle vermelde abonnementen bijwerken door het selecteren van **toepassen standaardplan**.

  ![Upgraden van alle abonnementen][1]

Een afzonderlijk abonnement bijwerken naar Standard: U kunt een abonnement van een upgrade **Onboarding** door te selecteren **standaardcategorie toepassen**. Als u wilt upgraden van een resourcegroep onder het abonnement op standaard, selecteer het abonnement:
1. Selecteer een abonnement.  **Beveiligingsbeleid** bevat informatie over de resourcegroep die is opgenomen in het abonnement.
2. Selecteer het abonnement of resourcegroep.

  ![Upgraden van alle abonnementen][2]

3. Selecteer **standaard** upgrade vanaf gratis naar standaard.
4. Selecteer **Opslaan**.

> [!NOTE]
> Upgraden van een abonnement op standaard wordt ingeschakeld [automatische inrichting](security-center-enable-data-collection.md) als deze eerder was uitgeschakeld. Het is raadzaam om automatische inrichting van de bewaking van agents.
>
>

## <a name="upgrade-a-workspace"></a>Het bijwerken van een werkruimte
Standaard wordt toegepast op de werkruimte geldt voor alle resources in de werkruimte rapportage.

1. Ga terug naar de **Onboarding** blade.
2. Selecteer een werkruimte.

  ![Het bijwerken van een werkruimte][8]

3. Selecteer **standaard** om bij te werken.  
4. Selecteer **Opslaan**.

   > [!NOTE]
   > Er is een scenario waarbij hebt u geen vrije of standaard toegepast op uw werkruimte. Als u vrije selecteert, worden de beschikbare mogelijkheden van Security Center toegepast op uw Azure VM's alleen. De beschikbare mogelijkheden worden niet toegepast op de niet-Azure-computers. Als u standaard selecteert, worden de mogelijkheden voor standaard toegepast op alle Azure VM's en niet-Azure-computers die rapporteren aan de werkruimte. U wordt aangeraden Standard voor geavanceerde beveiliging voor uw Azure- en niet-Azure-resources toe te passen.
   >
   >

## <a name="onboard-non-azure-computers"></a>Ingebouwde niet-Azure-computers
Security Center kan de beveiligingsstatus van uw niet-Azure-computers controleren, maar u moet deze resources dan eerst onboarden. U kunt niet-Azure computers toevoegen uit de **Onboarding** blade of vanuit de **Compute** blade. We doorlopen beide methoden.

### <a name="add-new-non-azure-computers-from-onboarding"></a>Nieuwe niet-Azure-computers toevoegen van Onboarding

1. Ga terug naar **Onboarding**.   
2. Selecteer **wilt u de nieuwe niet-Azure-computers toevoegen**.

  ![Niet-Azure-computer toevoegen][3]

Als u bestaande werkruimten hebt, worden vermeld in **nieuwe niet-Azure-computers toevoegen**. U kunt computers toevoegen aan een bestaande werkruimte of maak een nieuwe werkruimte. Selecteer de koppeling voor het maken van een nieuwe werkruimte **toevoegen van een nieuwe werkruimte**.

We doorlopen beide methoden:

- Maak een nieuwe werkruimte en de computer toevoegen
- Selecteer een bestaande werkruimte en computer toevoegen

**Maak een nieuwe werkruimte en de computer toevoegen**

1. Onder **nieuwe niet-Azure-computers toevoegen**, selecteer **toevoegen van een nieuwe werkruimte**.

   ![Een nieuwe werkruimte toevoegen][4]

2. Onder **beveiligings- en Audit**, selecteer **OMS-werkruimte** om een nieuwe werkruimte te maken.
3. Onder **OMS-werkruimte**, voer de gegevens voor uw werkruimte.
4. Onder **OMS-werkruimte**, selecteer **OK**.  Nadat u OK selecteert, ontvangt u een koppeling voor het downloaden van een Windows- of Linux-agent en de sleutels voor uw werkruimte-ID moet worden gebruikt in de configuratie van de agent.
5. Onder **beveiligings- en Audit**, selecteer **OK**.

**Selecteer een bestaande werkruimte en computer toevoegen**

U kunt een computer toevoegen aan de hand van de werkstroom vanaf **Onboarding**, zoals hierboven. U kunt ook een computer toevoegen aan de hand van de werkstroom vanaf **Compute**. In dit voorbeeld gebruiken we **Compute**.

1. Ga terug naar het hoofdmenu van het Beveiligingscentrum en **overzicht** dashboard.

   ![Overzicht][5]

2. Selecteer de **Compute** tegel.
3. Onder **Compute**, selecteer **computers toevoegen**.

   ![Blade berekenen][6]

4. Onder **nieuwe niet-Azure-computers toevoegen**, selecteer een werkruimte naar uw computer verbinden met en klik op **Computers toevoegen**.

   ![Computers toevoegen][7]

 De **Direct Agent** blade bevat een koppeling voor het downloaden van een Windows- of Linux-agent en -sleutels voor uw werkruimte-ID moet worden gebruikt in de configuratie van de agent.   

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u ingebouwde Azure en niet-Azure-resources om te profiteren van Security Center van geavanceerde beveiliging.  Als u wilt doen met Zie uw resources vrijgegeven,

- [Gegevensverzameling inschakelen](security-center-enable-data-collection.md)
- [Bedreigingsinformatierapport](security-center-threat-report.md)
- [Alleen bij het VM-time-toegang](security-center-just-in-time.md)

<!--Image references-->
[1]: ./media/security-center-onboarding/onboard.png
[2]: ./media/security-center-onboarding/onboard-subscription.png
[3]: ./media/security-center-onboarding/add-non-azure-resource.png
[4]: ./media/security-center-onboarding/create-workspace.png
[5]: ./media/security-center-onboarding/overview.png
[6]: ./media/security-center-onboarding/compute-blade.png
[7]: ./media/security-center-onboarding/add-non-azure-computer.png
[8]: ./media/security-center-onboarding/onboard-workspace.png

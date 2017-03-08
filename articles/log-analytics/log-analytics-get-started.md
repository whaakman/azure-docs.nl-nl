---
title: Aan de slag met een Azure Log Analytics-werkruimte | Microsoft Docs
description: U kunt binnen enkele minuten aan de slag met een Azure Log Analytics-werkruimte.
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 508716de-72d3-4c06-9218-1ede631f23a6
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/08/2017
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: f75386f970aeb5694d226cfcd569b8c04a253191
ms.openlocfilehash: 0f418af5728b6a156ebc72fb99a3d16d559654ed
ms.lasthandoff: 02/10/2017


---
# <a name="get-started-with-a-log-analytics-workspace"></a>Aan de slag met een Log Analytics-werkruimte
U kunt snel aan de slag met Azure Log Analytics. Deze service helpt u bij het evalueren van operationele gegevens die over uw IT-infrastructuur zijn verzameld. Aan de hand van dit artikel kunt u gemakkelijk beginnen met het verkennen, analyseren en benutten van gegevens die u *gratis* verzamelt.

Dit artikel fungeert als inleiding op Log Analytics. Het bevat een korte zelfstudie die u stapsgewijs laat zien hoe u een minimale implementatie in Azure uitvoert. U kunt de verworven kennis daarna meteen in de praktijk brengen. Een werkruimte is een logische container waarin uw beheergegevens in Azure worden opgeslagen. Nadat u deze informatie hebt doorgenomen en uw eigen evaluatie hebt voltooid, kunt u de evaluatiewerkruimte verwijderen. Omdat dit artikel een zelfstudie betreft, blijven specifieke bedrijfsbehoeften, planningsvereisten en architectuurrichtlijnen buiten beschouwing.

>[!NOTE]
>Als u de Microsoft Azure Government Cloud gebruikt, neemt u in plaats van dit artikel [Azure Government Monitoring + Management documentation](https://review.docs.microsoft.com/azure/azure-government/documentation-government-services-monitoringandmanagement#log-analytics) (Documentatie voor Azure Government Monitoring + Management) door.

Hier volgt een kort overzicht van het proces dat u doorloopt om aan de slag te gaan:

![procesdiagram](./media/log-analytics-get-started/onboard-oms.png)

## <a name="1-create-an-azure-account-and-sign-in"></a>1 Een Azure-account maken en u aanmelden

Als u nog geen Azure-account hebt, moet u er een maken om Log Analytics te kunnen gebruiken. U kunt een [gratis account](https://azure.microsoft.com/free/) maken dat 30 dagen geldig is en waarmee u toegang hebt tot alle Azure-services.

### <a name="to-create-a-free-account-and-sign-in"></a>Een gratis account maken en aanmelden
1. Volg de aanwijzingen in [Een gratis Azure-account maken](https://azure.microsoft.com/free/).
2. Ga naar [Azure Portal](https://portal.azure.com) en meld u aan.

## <a name="2-create-a-workspace"></a>2 Een werkruimte maken

De volgende stap is het maken van een werkruimte.

1. Ga in de Marketplace in Azure Portal naar de lijst met services voor *Log Analytics* en selecteer **Log Analytics**.  
    ![Azure Portal](./media/log-analytics-get-started/log-analytics-portal.png)
2. Klik op **Maken** en geef uw keuze aan voor de volgende items:
   * **OMS-werkruimte**: geef een naam op voor uw werkruimte.
   * **Abonnement**: als u meerdere abonnementen hebt, kiest u het abonnement dat u aan de nieuwe werkruimte wilt koppelen.
   * **Resourcegroep**
   * **Locatie**
   * **Prijscategorie**  
       ![snel maken](./media/log-analytics-get-started/oms-onboard-quick-create.png)
3. Klik op **OK** om een lijst met uw werkruimten weer te geven.
4. Selecteer een werkruimte om de details ervan in Azure Portal te bekijken.       
    ![details van de werkruimte](./media/log-analytics-get-started/oms-onboard-workspace-details.png)         

## <a name="3-add-solutions-and-solution-offerings"></a>3 Oplossingen en oplossingenpakketten toevoegen

Voeg vervolgens beheeroplossingen en oplossingenpakketten toe. Beheeroplossingen bestaan uit een combinatie van logica, visualisatiemogelijkheden en regels voor gegevensverwerving die metrische gegevens bieden met betrekking tot een specifiek probleemgebied. Een oplossingenpakket is een samenstelling van meerdere beheeroplossingen.

Als u oplossingen aan uw werkruimte toevoegt, kan Log Analytics diverse soorten gegevens verzamelen van computers die met uw werkruimte zijn verbonden. Hiervoor worden agents gebruikt. Het onboarden van deze agents komt later aan bod.

### <a name="to-add-solutions-and-solution-offerings"></a>Oplossingen en oplossingenpakketten toevoegen

1. Klik in Azure Portal op **Nieuw** en typ in het vak **Marketplace doorzoeken** de tekst **Activity Log Analytics**. Druk vervolgens op ENTER.
2. Selecteer op de blade Alles de optie **Activity Log Analytics** en klik op **Maken**.  
    ![Activity Log Analytics](./media/log-analytics-get-started/activity-log-analytics.png)  
3. Selecteer op de blade *Naam beheeroplossing* de werkruimte die u aan de beheeroplossing wilt koppelen.
4. Klik op **Maken**.  
    ![werkruimte voor de oplossing](./media/log-analytics-get-started/solution-workspace.png)  
5. Herhaal stappen 1-4 om het volgende toe te voegen:
    - Het servicepakket **Security & Compliance** met de oplossingen Antimalware Assessment en Security and Audit.
    - Het servicepakket **Automation & Control** met de oplossingen Automation Hybrid Worker, 	Change Tracking en System Update Assessment (ook wel Update Management genoemd). Wanneer u een servicepakket toevoegt, moet u een Automation-account maken.  
        ![Automation-account](./media/log-analytics-get-started/automation-account.png)  
6. U kunt de beheeroplossingen die u aan uw werkruimte hebt toegevoegd, weergeven door te navigeren naar **Log Analytics** > **Abonnementen** > ***Werkruimtenaam*** > **Overzicht**. Er worden nu tegels weergegeven voor de beheeroplossingen die u hebt toegevoegd.  
    >[!NOTE]
    >Omdat we nog geen agents aan de werkruimte hebben gekoppeld, ziet u geen gegevens voor de oplossingen die u hebt toegevoegd.  

    ![oplossingstegels zonder gegevens](./media/log-analytics-get-started/solutions-no-data.png)

## <a name="4-create-a-vm-and-onboard-an-agent"></a>4 Een virtuele machine maken en een agent onboarden

Maak vervolgens een eenvoudige virtuele machine in Azure. Nadat u een virtuele machine hebt gemaakt, onboardt u de OMS-agent om deze in te schakelen. Door het inschakelen van de agent begint het verzamelen van gegevens vanaf de virtuele machine en worden er gegevens naar Log Analytics verzonden.

### <a name="to-create-a-virtual-machine"></a>Een virtuele machine maken

- Volg de aanwijzingen in [Uw eerste virtuele Windows-machine maken met behulp van Azure Portal](../virtual-machines/virtual-machines-windows-hero-tutorial.md) en start de nieuwe virtuele machine.

### <a name="connect-the-virtual-machine-to-log-analytics"></a>De virtuele machine verbinden met Log Analytics

- Volg de aanwijzingen in [Connect Azure virtual machines to Log Analytics](log-analytics-azure-vm-extension.md) (Virtuele Azure-machines verbinden met Log Analytics) om de virtuele machine te verbinden met Log Analytics met behulp van Azure Portal.

## <a name="5-view-and-act-on-data"></a>5 Gegevens weergeven en er actie op ondernemen

U hebt de oplossing Activity Log Analytics en de servicepakketten Security & Compliance en Automation & Control al ingeschakeld. Nu gaan we kijken naar de gegevens die door deze oplossingen zijn verzameld en naar de resultaten van zoeken in logboeken.

Bekijk eerst de gegevens die vanuit de oplossingen zijn aangeleverd. Bekijk vervolgens de resultaten van enkele zoekopdrachten in logboeken. Met zoeken in logboeken kunt u alle machinegegevens vanuit meerdere bronnen binnen uw omgeving combineren en correleren. Zie [Log searches in Log Analytics](log-analytics-log-searches.md) (Zoeken in logboeken via Log Analytics) voor meer informatie. Neem tot slot actie op de gegevens die we hebben gevonden. Gebruik hiervoor de OMS-portal, die zich buiten Azure Portal bevindt.

### <a name="to-view-antimalware-data"></a>Gegevens van Antimalware Assessment weergeven

1. Navigeer in Azure Portal naar **Log Analytics** > ***uw werkruimte***.
2. Klik op de blade voor uw werkruimte onder **Algemeen** op **Overzicht**.  
    ![Overzicht](./media/log-analytics-get-started/overview.png)
3. Klik op de tegel **Antimalware Assessment**. In dit voorbeeld ziet u dat op één computer Windows Defender is geïnstalleerd, maar dat de handtekening is verouderd.  
    ![Antimalware](./media/log-analytics-get-started/solution-antimalware.png)
4. Voor dit voorbeeld klikt u onder **Beveiligingsstatus** op **Handtekening verouderd** om Zoeken in logboeken te openen en details weer te geven over de computers die verouderde handtekeningen hebben. In dit voorbeeld heeft de computer de naam *getstarted*. Als er meer dan één computer met verouderde handtekeningen zou zijn, zouden deze allemaal worden weergegeven in de resultaten van Zoeken in logboeken.  
    ![Antimalware verouderd](./media/log-analytics-get-started/antimalware-search.png)

### <a name="to-view-security-and-audit-data"></a>Gegevens van Security and Audit weergeven

1. Klik op de blade voor uw werkruimte onder **Algemeen** op **Overzicht**.  
2. Klik op de tegel **Security and Audit**. In dit voorbeeld ziet u dat er twee belangrijke problemen zijn: er is een computer waarop essentiële updates ontbreken en er is een computer met onvoldoende beveiliging.  
    ![Beveiliging en controle](./media/log-analytics-get-started/security-audit.png)
3. Voor dit voorbeeld klikt u onder **Problemen die aandacht vereisen** op **Computers waarop essentiële updates ontbreken** om Zoeken in logboeken te openen en meer informatie weer te geven over computers waarop essentiële updates ontbreken. In dit voorbeeld ontbreekt er één essentiële update, plus nog 63 andere updates.  
    ![Zoeken in logboeken met betrekking tot beveiliging en controle](./media/log-analytics-get-started/security-audit-log-search.png)

### <a name="to-view-and-act-on-system-update-data"></a>Systeemupdategegevens weergeven en er actie op ondernemen

1. Klik op de blade voor uw werkruimte onder **Algemeen** op **Overzicht**.  
2. Klik op de tegel **System Update Assessment**. In dit voorbeeld ziet u dat er een Windows-computer is met de naam *getstarted* die essentiële updates vereist en een computer die definitie-updates vereist.  
    ![Systeemupdates](./media/log-analytics-get-started/system-updates.png)
3. Voor dit voorbeeld klikt u onder **Ontbrekende updates** op **Essentiële updates** om Zoeken in logboeken te openen en geeft u details weer over computers waarop essentiële updates ontbreken. In dit voorbeeld ontbreekt één update en is er één vereiste update.  
    ![Zoeken in logboeken met betrekking tot systeemupdates](./media/log-analytics-get-started/system-updates-log-search.png)
4. Ga naar de [Operations Management Suite](http://microsoft.com/oms)-website en meld aan met uw Azure-account. Wanneer u bent aangemeld, ziet u hier vrijwel identieke informatie over de oplossing als in Azure Portal.  
    ![OMS-portal](./media/log-analytics-get-started/oms-portal.png)
5. Klik op de tegel **Update Management**.
6. Op het dashboard Update Management ziet u dat de systeemupdate-informatie identiek is aan die in Azure Portal. De tegel **Manage Update Deployments** is echter nieuw. Klik op de tegel **Manage Update Deployments**.  
    ![Tegel Update Management](./media/log-analytics-get-started/update-management.png)
7. Klik op de pagina **Update Deployments** op **Add** om een *update uit te voeren*.  
    ![Update-implementaties](./media/log-analytics-get-started/update-management-update-deployments.png)
8.  Geef op de pagina **New Update Deployment** een naam op voor de update-implementatie, selecteer de computers die u wilt bijwerken (in dit voorbeeld *getstarted*), kies een planning en klik vervolgens op **Save**.  
    ![Nieuwe implementatie](./media/log-analytics-get-started/new-deployment.png)  
    Nadat u de update-implementatie hebt opgeslagen, ziet u de geplande update.  
    ![geplande update](./media/log-analytics-get-started/scheduled-update.png)  
    Nadat de update is uitgevoerd, wordt de status **Finished** weergegeven.
    ![voltooide update](./media/log-analytics-get-started/completed-update.png)
9. Nadat de update is uitgevoerd, kunt u nagaan of de implementatie is gelukt en kunt u details over de toegepaste updates weergeven.

## <a name="after-evaluation"></a>Evaluatie van deze zelfstudie

In deze zelfstudie hebt u een agent geïnstalleerd op een virtuele machine en bent u snel aan de slag gegaan. De stappen die u hebt gevolgd, waren snel en eenvoudig. De meeste grote organisaties en ondernemingen hebben echter complexe on-premises IT-infrastructuren. Het verzamelen van gegevens over deze complexe omgevingen vereist aanzienlijk meer planning en inspanning dan nodig waren voor deze zelfstudie. In het gedeelte Volgende stappen hieronder vindt u koppelingen naar nuttige artikelen.

Desgewenst kunt u de werkruimte die u in deze zelfstudie hebt gemaakt, nu verwijderen.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het verbinden van [Windows-agents](log-analytics-windows-agents.md) met Log Analytics.
* Meer informatie over het verbinden van [Operations Manager-agents](log-analytics-om-agents.md) met Log Analytics.
* [Log Analytics-oplossingen uit de galerie met oplossingen toevoegen](log-analytics-add-solutions.md) om functionaliteit toe te voegen en gegevens te verzamelen.
* Vertrouwd raken met [zoekopdrachten naar logboeken](log-analytics-log-searches.md) om gedetailleerde informatie te bekijken die is verzameld door oplossingen.


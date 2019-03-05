---
title: Een omgeving maken met een Service Fabric-cluster in Azure DevTest Labs | Microsoft Docs
description: Leer hoe u een omgeving met een zelfstandige Service Fabric-cluster maken en starten en stoppen van het cluster met behulp van schema's.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: EMaher
manager: femila
editor: spelluru
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/01/2019
ms.author: enewman
ms.openlocfilehash: 9848f197800c391285c4065685b910685f0ac64b
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57318787"
---
# <a name="create-an-environment-with-self-contained-service-fabric-cluster-in-azure-devtest-labs"></a>Maken van een omgeving met zelfstandige Service Fabric-cluster in Azure DevTest Labs
In dit artikel bevat informatie over het maken van een omgeving met een zelfstandige Service Fabric-cluster in Azure DevTest Labs. 

## <a name="overview"></a>Overzicht
DevTest Labs kunt zelfstandig en testomgevingen maken, zoals gedefinieerd door Azure Resource Management-sjablonen. Deze omgevingen bevatten beide IaaS-resources, zoals virtual machines en PaaS-resources, zoals Service Fabric. DevTest Labs kunt u virtuele machines in een omgeving beheren door op te geven van de opdrachten voor het beheren van de virtuele machines. Deze opdrachten geeft u de mogelijkheid om te starten of stoppen van een virtuele machine volgens een schema. Op deze manier kunt DevTest Labs u ook Service Fabric-clusters in een omgeving te beheren. U kunt starten of stoppen van een Service Fabric-cluster in een omgeving handmatig of via een planning.

## <a name="create-a-service-fabric-cluster"></a>Een Service Fabric-cluster maken
Service Fabric-clusters worden gemaakt met behulp van de omgevingen in DevTest Labs. Elke omgeving wordt gedefinieerd door een Azure Resource Manager-sjabloon in een Git-opslagplaats. De [openbare Git-opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Environments/) voor DevTest Labs, de Resource Manager-sjabloon bevat voor het maken van een Service Fabric-cluster in de [service fabric-Cluster](https://github.com/Azure/azure-devtestlab/tree/master/Environments/ServiceFabric-LabCluster) map. 

1. Maak eerst een lab in Azure DevTest Labs met behulp van instructies in het volgende artikel: [Een lab maken](devtest-lab-create-lab.md). U ziet dat de **openbare omgevingen** optie **op** standaard. 
2. Bevestig dat de Service Fabric-provider is geregistreerd voor uw abonnement door de volgende stappen:
    1. Selecteer **abonnementen** in het navigatiemenu links en selecteer uw **abonnement**
    2. Op de **abonnement** weergeeft, schakelt **resourceproviders** in de **instellingen** sectie in het menu links. 
    3. Als **Microsoft.ServiecFabric** niet is geregistreerd, selecteer **registreren**. 
3. Op de **DevTest Lab** -pagina voor uw lab, selecteer **+ toevoegen** op de werkbalk. 
    
    ![Knop toevoegen op de werkbalk](./media/create-environment-service-fabric-cluster/add-button.png)
3. Op de **kiest u een base** weergeeft, schakelt **Service Fabric-Cluster Lab** in de lijst. 

    ![Service Fabric-Cluster Lab in de lijst selecteren](./media/create-environment-service-fabric-cluster/select-service-fabric-cluster.png)
4. Op de **-instellingen configureren** pagina, de volgende stappen uit: 
    1. Geef een **naam** voor uw cluster **omgeving**. Dit is de naam van de resourcegroep in Azure waarin de Service Fabric-cluster zal worden gemaakt. 
    2. Selecteer de **besturingssysteem (OS)** voor het cluster virtuele machines. De standaardwaarde is: **Windows**.
    3. Geef een naam voor de **beheerder** voor het cluster. 
    4. Geef een **wachtwoord** voor de beheerder. 
    5. Voor de **certificaat**, voert u uw certificaat als een met Base64 gecodeerde tekenreeks. Voor het maken van een certificaat, voer de volgende stappen uit:
        1. Download de **maken ClusterCertificate.ps1** -bestand uit de [Git-opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Environments/ServiceFabric-LabCluster). U kunt ook kloon de opslagplaats op uw computer. 
        2. Start **PowerShell**. 
        3. Voer de **ps1** bestand met de opdracht `.\Create-ClusterCertificate.ps1`. U ziet een tekstbestand geopend in Kladblok met informatie die u nodig hebt om in te vullen in de certificaat-gerelateerde velden op deze pagina. . 
    6. Voer de **wachtwoord voor het certificaat**.
    7. Geef de **vingerafdruk** voor uw certificaat.
    8. Selecteer **toevoegen** op de **-instellingen configureren** pagina. 

        ![Clusterinstellingen configureren](./media/create-environment-service-fabric-cluster/configure-settings.png)
5. Nadat het cluster is gemaakt, ziet u een resourcegroep met de naam van de omgeving die u hebt opgegeven in de vorige stap. Als u wilt uitbreiden, ziet u de Service Fabric-cluster in deze. Als de status van de resourcegroep is vastgelopen bij **maken**, selecteer **vernieuwen** op de werkbalk. De **Service Fabric-cluster** omgeving maakt u een cluster met 5-knooppunt 1-nodetype in Linux of Windows.

    In het volgende voorbeeld **mysfabricclusterrg** is de naam van de resourcegroep die specifiek voor de Service Fabric-cluster gemaakt. Het is belangrijk te weten dat lab-omgevingen zijn ingesloten in de resourcegroep waarin ze zijn gemaakt. Dit betekent dat de sjabloon die de omgeving, die alleen toegang resources in de zojuist gemaakte resource-groep tot definieert of [virtuele netwerken die zijn geconfigureerd om te worden gebruikt door het lab](devtest-lab-configure-vnet.md). In dit voorbeeld hierboven maakt de vereiste resources in dezelfde resourcegroep bevinden.

    ![Cluster dat is gemaakt](./media/create-environment-service-fabric-cluster/cluster-created.png)

## <a name="start-or-stop-the-cluster"></a>Starten of stoppen van het cluster
U kunt starten of stoppen van het cluster via de de DevTest Lab-pagina zelf of op de pagina Service Fabric-Cluster is geleverd door DevTest Labs. 

### <a name="from-the-devtest-lab-page"></a>Op de pagina DevTest Lab
U kunt starten of stoppen van het cluster op de pagina DevTest Lab voor uw testomgeving. 

1. Selecteer **drie puntjes (...)**  voor de Service Fabric-cluster, zoals wordt weergegeven in de volgende afbeelding: 

    ![Starten en stoppen van de opdrachten voor het cluster](./media/create-environment-service-fabric-cluster/start-stop-on-devtest-lab-page.png)

2. U ziet twee opdrachten in het contextmenu **start** en **stoppen** het cluster. De opdracht start Start alle knooppunten in een cluster. De opdracht stop stopt alle knooppunten in een cluster. Nadat een cluster is gestopt, wordt het Service Fabric-cluster zelf blijft aanwezig in een status gereed heeft, maar er zijn geen knooppunten zijn beschikbaar totdat de opdracht start opnieuw op het cluster in de testomgeving is uitgegeven.

    Er zijn enkele overwegingen om te weten wanneer u een Service Fabric-cluster in een testomgeving. Het duurt even voordat de Service Fabric-cluster volledig rehydrate nadat de knooppunten opnieuw zijn opgestart. Om gegevens te behouden van afsluiten om te starten, moeten de gegevens worden opgeslagen op een beheerde schijf die is gekoppeld aan de virtuele machine. Zijn er gevolgen voor de prestaties bij het gebruik van een gekoppelde beheerde schijf, zodat het wordt aanbevolen voor testomgevingen alleen. Als de schijf die wordt gebruikt voor de opslag van gegevens wordt niet ondersteund, klikt u vervolgens gaat gegevens verloren wanneer de opdracht stoppen is uitgevoerd op het cluster.

### <a name="from-the-service-fabric-cluster-page"></a>Op de pagina Service Fabric-Cluster 
Er is een andere manier om te starten of stoppen van het cluster. 

1. Selecteer uw Service Fabric-cluster in de structuurweergave wordt weergegeven op de pagina DevTest Lab. 

    ![Selecteer uw cluster](./media/create-environment-service-fabric-cluster/select-cluster.png)

2. Op de **Service Fabric-Cluster** pagina voor het cluster, ziet u opdrachten op de werkbalk om te starten of stoppen van het cluster. 

    ![Starten of stoppen van opdrachten op de pagina met Service Fabric-Cluster](./media/create-environment-service-fabric-cluster/start-stop-on-cluster-page.png)

## <a name="configure-auto-startup-and-auto-shutdown-schedule"></a>Automatisch opstarten en planning voor automatische afsluiting configureren
Service Fabric-clusters kunnen ook worden gestart of gestopt volgens een schema. Deze ervaring is vergelijkbaar met de ervaring voor virtuele machines in een testomgeving. Om geld te besparen, standaard, elk cluster is gemaakt in een testomgeving automatisch afgesloten op het moment dat is gedefinieerd door het lab [afsluiten beleid](devtest-lab-get-started-with-lab-policies.md#set-auto-shutdown). U kunt onderdrukken door op te geven of het cluster moet worden afgesloten of door het opgeven van de tijd die het cluster wordt afgesloten. 

![Bestaande schema's voor automatisch starten en automatisch afsluiten](./media/create-environment-service-fabric-cluster/existing-schedules.png)

### <a name="opt-in-to-the-auto-start-schedule"></a>U meldt zich aan de planning voor automatische start
Als u wilt deelnemen aan de planning voor opstarten, moet u de volgende stappen uitvoeren:

1. Selecteer **automatisch starten** in het menu links
2. Selecteer **op** voor **toestaan dat deze service fabric-cluster moet worden gepland voor automatisch starten**. Deze pagina is alleen ingeschakeld als de eigenaar van het lab gebruikers automatisch starten is toegestaan, hun virtuele machines of de Service Fabric-clusters.
3. Selecteer **Opslaan** op de werkbalk. 

    ![Automatisch star-pagina](./media/create-environment-service-fabric-cluster/set-auto-start-settings.png)

### <a name="configure-auto-shutdown-settings"></a>Configureren van instellingen voor automatisch afsluiten 
Wijzigen van instellingen voor het afsluiten van de volgende stappen uitvoeren:

1. Selecteer **Auto-shutdown** in het menu links. 
2. Op deze pagina kunt u automatisch afsluiten afmelden hiervoor **uit** voor **ingeschakeld**. 
3. Als u hebt geselecteerd **op** voor **ingeschakeld**, als volgt te werk:
    1. Geef de **tijd** voor afsluiten.
    2. Geef de **tijdzone** voor de tijd. 
    3. Geef op of u DevTest Labs wilt voor het verzenden van **meldingen** voor automatisch afsluiten. 
    4. Als u hebt geselecteerd **Ja** voor de meldingsoptie, geef de **Webhook-URL** en/of **e-mailadres** om meldingen te verzenden. 
    5. Selecteer **Opslaan** op de werkbalk.

        ![Automatisch afsluiten van pagina](./media/create-environment-service-fabric-cluster/auto-shutdown-settings.png)

## <a name="to-view-nodes-in-the-service-fabric-cluster"></a>Knooppunten in het Service Fabric-cluster weergeven
De Service Fabric-cluster-pagina die u eerder hebt gezien in de stappen is specifiek voor de DevTest Labs-pagina. Het niet laten zien u de knooppunten in het cluster. Als u meer informatie over het cluster, de volgende stappen uit:

1. Op de **DevTest Lab** -pagina voor uw lab, selecteer de **resourcegroep** in de structuurweergave wordt weergegeven in de **mijn virtuele machines** sectie.

    ![Resourcegroep selecteren](./media/create-environment-service-fabric-cluster/select-resource-group.png)
2. Op de **resourcegroep** pagina, ziet u alle resources in de resourcegroep in een lijst. Selecteer uw **Service Fabric-cluster** in de lijst. 

    ![Selecteer uw cluster in de lijst](./media/create-environment-service-fabric-cluster/select-cluster-resource-group-page.png)
3. U ziet de **Service Fabric-Cluster** pagina voor uw cluster. Dit is de pagina waarop de Service Fabric biedt. Ziet u alle informatie over de clusters, zoals knooppunten, knooppunttypen, enzovoort.

    ![Startpagina voor service Fabric-Cluster](./media/create-environment-service-fabric-cluster/service-fabric-cluster-page.png)

## <a name="next-steps"></a>Volgende stappen
Zie de volgende artikelen voor meer informatie over omgevingen: 

- [Multi-VM-omgevingen en PaaS-resources maken met Azure Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md)
- [Configuratie en het gebruik van openbare omgevingen in Azure DevTest Labs](devtest-lab-configure-use-public-environments.md)

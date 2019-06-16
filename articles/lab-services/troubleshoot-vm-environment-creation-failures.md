---
title: Problemen oplossen met virtuele machine en de omgeving maken mislukte Azure DevTest Labs | Microsoft Docs
description: Informatie over het oplossen van de virtuele machine (VM) en fouten bij het maken in Azure DevTest Labs omgeving.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2019
ms.author: spelluru
ms.openlocfilehash: 7baa5e4c113e6c21c6123ac7c8399533a7dfb358
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65410305"
---
# <a name="troubleshoot-virtual-machine-vm-and-environment-creation-failures-in-azure-devtest-labs"></a>Problemen oplossen met virtuele machine (VM) en fouten bij het maken van omgeving in Azure DevTest Labs
DevTest Labs kunt u waarschuwingen als een computernaam ongeldig is of als u over naar een lab-beleid schenden. Soms ziet u red `X` naast uw lab status van de virtuele machine of omgeving waarmee u wordt geïnformeerd dat er iets misgegaan is.  Dit artikel bevat een aantal tips die u gebruiken kunt om te zoeken van het onderliggende probleem en, natuurlijk het probleem te voorkomen in de toekomst.

## <a name="portal-notifications"></a>Portalmeldingen
Als u van de Azure-portal gebruikmaakt, de eerste plaats om te kijken, is de **meldingenvenster**.  De meldingen van het deelvenster, beschikbaar op de opdrachtbalk door te klikken op de **belpictogram**, laat u weten of het lab maken van virtuele machine of de omgeving is geslaagd of mislukt.  Als er een fout is opgetreden, ziet u het foutbericht dat is gekoppeld aan het maken is mislukt. De details geven vaak meer informatie over het probleem is opgelost. In het volgende voorbeeld is het maken van de virtuele machine is mislukt vanwege het opraakt kernen. Het gedetailleerde bericht wordt uitgelegd hoe u los het probleem en een core verhoging aanvragen.

![Melding in Azure portal](./media/troubleshoot-vm-environment-creation-failures/portal-notification.png)


## <a name="activity-logs"></a>Activiteitenlogboeken
Activiteitenlogboeken bekijken als u een fout onderzoekt enige tijd opnieuw uit wanneer u zich probeert het maken van uw virtuele machine of de omgeving. Deze sectie leest u hoe u Logboeken voor VM's en omgevingen vinden.

## <a name="activity-logs-for-virtual-machines"></a>Activiteitenlogboeken voor virtuele machines

1. Selecteer de virtuele machine te starten op de startpagina van uw testomgeving, de **virtuele Machine** pagina.
2. Op de **virtuele Machine** pagina, in de **bewaking** gedeelte van het menu links, selecteer **activiteitenlogboek** om te zien van alle logboeken die zijn gekoppeld aan de virtuele machine.
3. Selecteer in de activiteit logboekitems, de bewerking die is mislukt. Normaal gesproken de mislukte bewerking heet `Write Virtualmachines`.
4. Overschakelen naar het tabblad JSON in het rechter deelvenster. U ziet de details in de JSON-weergave van het logboek.

    ![Activiteitenlogboek voor een virtuele machine](./media/troubleshoot-vm-environment-creation-failures/vm-activity-log.png)
5. Het JSON-logboek te controleren totdat u de `statusMessage` eigenschap. Dit biedt u de belangrijkste foutbericht en meer gedetailleerde informatie, indien van toepassing. De volgende JSON is een voorbeeld van de belangrijkste tussen aanhalingstekens overschreden-fout die eerder in dit artikel.

    ```json
    "properties": {
        "statusCode": "Conflict",
        "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"The resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"OperationNotAllowed\",\"message\":\"Operation results in exceeding quota limits of Core. Maximum allowed: 100, Current in use: 100, Additional requested: 8. Please read more about quota increase at https://aka.ms/corequotaincrease.\"}]}}",
    },
    ```

## <a name="activity-log-for-an-environment"></a>Activiteitenlogboek voor een omgeving

Als u wilt zien van het activiteitenlogboek voor het maken van een omgeving, de volgende stappen uit:

1. Selecteer op de startpagina van uw lab, **configuratie en het beleid** in het menu links.
2. op de **configuratie en het beleid** weergeeft, schakelt **activiteitenlogboeken** in het menu.
3. De fout in de activiteitenlijst in het logboek zoekt en selecteert u deze.
4. Ga naar het tabblad JSON in het rechter deelvenster en zoekt de **statusMessage**.

    ![Activiteitenlogboek-omgeving](./media/troubleshoot-vm-environment-creation-failures/envirionment-activity-log.png)

## <a name="resource-manager-template-deployment-logs"></a>Implementatielogboeken voor Resource Manager-sjabloon
Als uw omgeving of de virtuele machine is gemaakt met behulp van automatisering, is er één laatste plaats om te controleren of de foutgegevens. Dit is het implementatielogboek van de Azure Resource Manager-sjabloon. Wanneer u een lab-resource is gemaakt met behulp van automatisering, wordt dit vaak gedaan door de sjabloonimplementatie van een Azure Resource Manager. Zie[ https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates ](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/QuickStartTemplates) voor voorbeeld van Azure Resource Manager-sjablonen die DevTest Labs-resources maken.

Als u wilt zien de implementatielogboeken voor lab-sjabloon, de volgende stappen uit:

1. Start de pagina voor de resourcegroep waarin het lab bestaat.
2. Selecteer **implementaties** in het menu links onder **instellingen**.
3. Implementaties met een mislukte status zoekt en selecteert u deze.
4. Op de **implementatie** weergeeft, schakelt **Bewerkingsdetails** koppeling voor de bewerking die is mislukt.
5. Ziet u meer informatie over de bewerking die is mislukt in de **Bewerkingsdetails** venster.

## <a name="next-steps"></a>Volgende stappen
Zie [problemen met artefacten fouten oplossen](devtest-lab-troubleshoot-artifact-failure.md)
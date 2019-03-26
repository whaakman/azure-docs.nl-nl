---
title: Een gegevensfactory installatiekopie uitvoeren vanaf Azure DevOps in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het maken van een aangepaste installatiekopie-factory in Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 62cb8222b592660f2e7ab32d438fd4073246ee50
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439824"
---
# <a name="run-an-image-factory-from-azure-devops"></a>Een gegevensfactory installatiekopie uitvoeren vanaf Azure DevOps
In dit artikel bevat informatie over alle de voorbereidingen die nodig zijn voor het uitvoeren van de factory-afbeelding van Azure DevOps (voorheen Visual Studio Team Services).

> [!NOTE]
> Een orchestration-engine werkt! Azure DevOps is niet verplicht. De factory installatiekopie wordt uitgevoerd met behulp van Azure PowerShell-scripts, zodat deze worden kan handmatig uitvoeren met behulp van Windows Taakplanner, andere CI/CD-systemen, enzovoort.

## <a name="create-a-lab-for-the-image-factory"></a>Een lab maken voor de installatiekopie-factory
De eerste stap bij het instellen van de installatiekopie factory is het maken van een lab in Azure DevTest Labs. In dit testlab wordt de afbeelding factory lab waar we de virtuele machines maken en opslaan van aangepaste installatiekopieën. In dit lab wordt beschouwd als onderdeel van het algehele proces voor de installatiekopie van factory. Zodra u een lab hebt gemaakt, zorg ervoor dat de naam worden opgeslagen omdat u hebt deze later nodig.

## <a name="scripts-and-templates"></a>Scripts en -sjablonen
De volgende stap in de fabriek installatiekopie voor uw team vast te stellen is om te begrijpen wat er beschikbaar is. De afbeelding factory scripts en -sjablonen zijn beschikbaar openbaar in de [DevTest Labs GitHub-opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Scripts/ImageFactory). Hier volgt een overzicht van de onderdelen:

- Afbeelding Factory. Het is de hoofdmap. 
    - De configuratie. De invoer voor de installatiekopie-factory
        - GoldenImages. Deze map bevat JSON-bestanden die staan voor de definities van aangepaste installatiekopieën.
        - Labs.json. Het bestand waarin teams zich registreren voor het ontvangen van specifieke aangepaste installatiekopieën.
- Scripts. De engine voor de factory installatiekopie.

De artikelen in deze sectie vindt u meer informatie over deze scripts en sjablonen. 

## <a name="create-an-azure-devops-team-project"></a>Een Azure DevOps team-project maken
Azure DevOps kunt u de broncode opslaan, voert u de Azure PowerShell op één plek. U kunt plannen dat periodiek wordt uitgevoerd installatiekopieën om up-to-date te houden. Er zijn goede voorzieningen voor het vastleggen van de resultaten om eventuele problemen te diagnosticeren.  U kunt alle kabelboom /-engine die verbinding kunnen maken met Azure en Azure PowerShell kunt uitvoeren met behulp van Azure DevOps is echter geen vereiste, gebruiken.

Als u een bestaande DevOps-account of een project dat u wilt gebruiken in plaats daarvan hebt, kunt u deze stap overslaan.

Als u wilt beginnen, moet u een gratis account maken in Azure DevOps. Ga naar https://www.visualstudio.com/ en selecteer **voor gratis aan de slag** direct onder **Azure DevOps** (voorheen VSTS). U moet een unieke naam kiezen en zorg ervoor dat u kiest voor het beheren van code met Git. Nadat deze is gemaakt, sla de URL naar uw teamproject. Hier volgt een voorbeeld-URL: https://<accountname>.visualstudio.com/MyFirstProject.

## <a name="check-in-the-image-factory-to-git"></a>Controleer in de factory, de installatiekopie met Git
Alle PowerShell, sjablonen en configuratie voor de factory installatiekopie bevinden zich in de [openbare DevTest Labs GitHub-opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Scripts/ImageFactory). De snelste manier om op te halen van de code in uw nieuwe teamproject is voor het importeren van een opslagplaats. Hiermee haalt de hele DevTest Labs-opslagplaats (je krijgt dus extra docs en voorbeelden). 

1. Ga naar het Azure DevOps-project dat u in de vorige stap hebt gemaakt (URL eruitziet **https://<accountname>.visualstudio.com/MyFirstProject**).
2. Selecteer **importeren van een opslagplaats**.
3. Voer de **kloon-URL** voor de DevTest Labs-opslagplaats: `https://github.com/Azure/azure-devtestlab`.
4. Selecteer **importeren**.

    ![Git-repo importeren](./media/set-up-devops-lab/import-git-repo.png)

Als u besluit om in te checken alleen precies wat u nodig hebt (de factory afbeeldingsbestanden), volgt u de stappen [hier](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs) om te klonen van de Git-opslagplaats en push-alleen de bestanden in de **scripts/ImageFactory** directory.

## <a name="create-a-build-and-connect-to-azure"></a>Een build maken en verbinding maken met Azure
Op dit moment, beschikt u over de bronbestanden die zijn opgeslagen in een Git-opslagplaats in Azure DevOps. Nu moet u het instellen van een pijplijn om uit te voeren van de Azure PowerShell. Er zijn tal van opties voor deze stappen uitvoeren. In dit artikel voor het gebruik van de build-definitie voor het gemak, maar het werkt met Build voor DevOps, DevOps-Release (één of meerdere omgevingen), andere uitvoering-engines, zoals Windows Taakplanner of andere kabelboom dat Azure PowerShell kan worden uitgevoerd.

> [!NOTE]
> Een belangrijk punt om Houd er rekening mee dat sommige van de PowerShell-bestanden erg lang duren om uit te voeren wanneer er veel (10 en hoger) aangepaste installatiekopieën te maken. Gratis gehoste DevOps Build en Release-agents hebt is een time-out van 30 minuten, zodat u kunt de gratis gehoste agent kan niet gebruiken wanneer u begint met het bouwen van vele installatiekopieën. Deze time-out uitdaging is van toepassing ongeacht kabelboom die u wilt gebruiken, is het goed om te controleren van voorgrond dat u de standaard time-outs voor langlopende Azure PowerShell-scripts kunt uitbreiden. In het geval van Azure DevOps, kunt u gebruik van betaalde gehoste Agents of gebruik uw eigen bouwagent.

1. Selecteer eerst **Build instellen** op de startpagina van uw DevOps-Project:

    ![Build-knop Instellingen](./media/set-up-devops-lab/setup-build-button.png)
2. Geef een **naam** voor de build (bijvoorbeeld: Bouwen en leveren van afbeeldingen bij Azure DevTest Labs). 
3. Selecteer een **leeg** build-definitie en selecteer **toepassen** te maken van uw build. 
4. In deze fase, kunt u **gehoste** voor de build-agent. 
5. **Sla** de build-definitie.

    ![Definitie van de build](./media/set-up-devops-lab/build-definition.png)

## <a name="configure-the-build-variables"></a>Configureren van de build-variabelen
Ter vereenvoudiging van de opdrachtregelparameters, bevatten de sleutelwaarden waarmee u de installatiekopie factory naar een set van build-variabelen uitbreiden. Selecteer de **variabelen** tabblad en u ziet een lijst met meerdere standaardvariabelen. Hier volgt de lijst met variabelen in te voeren in Azure devops:


| De naam van variabele | Value | Opmerkingen |
| ------------- | ----- | ----- |
| ConfigurationLocation | /Scripts/ImageFactory/Configuration | Dit is het volledige pad in de opslagplaats naar de **configuratie** map. Als u de bovenstaande hele opslagplaats hebt geïmporteerd, wordt de waarde aan de linkerkant juist is. Anders bijwerken om te verwijzen naar de locatie van de configuratie. |
| DevTestLabName | MyImageFactory | De naam van het lab in Azure DevTest Labs gebruikt omdat de factory voor het produceren van afbeeldingen. Als u niet hebt, maakt u een. Zorg ervoor dat de testomgeving in hetzelfde abonnement als dat het service-eindpunt toegang tot heeft. |
| ImageRetention | 1 | Het aantal installatiekopieën die u wilt opslaan voor elk type. Standaardwaarde ingesteld op 1. |
| MachinePassword | ******* | Ingebouwde wachtwoord voor het beheerdersaccount voor de virtuele machines. Dit is een tijdelijke account, dus zorg ervoor dat deze beveiligd worden. Selecteer het kleine vergrendelingspictogram aan de rechterkant om ervoor te zorgen dat deze een beveiligde tekenreeks. |
| MachineUserName | ImageFactoryUser | De ingebouwde Administrator-account-gebruikersnaam voor de virtuele machines. Dit is een tijdelijke account. |
| StandardTimeoutMinutes | 30 | De time-out voor die we voor normale werking van Azure wachten moet. |
| SubscriptionId |  0000000000-0000-0000-0000-0000000000000 | De ID van het abonnement waarin het lab bestaat en dat het service-eindpunt heeft toegang tot. |
| VMSize | Standard_A3 | De grootte van de virtuele machine moet worden gebruikt voor de **maken** stap. De virtuele machines die zijn gemaakt, zijn tijdelijke. De grootte moet het account dat [ingeschakeld voor de testomgeving](devtest-lab-set-lab-policy.md). Controleer of er voldoende [quotum voor kerngeheugens van abonnement](../azure-subscription-service-limits.md). 

![Variabelen maken](./media/set-up-devops-lab/configure-build-variables.png)

## <a name="connect-to-azure"></a>Verbinding maken met Azure
De volgende stap is het instellen van de service principal. Dit is een identiteit in Azure Active Directory waarmee de DevOps-build-agent werkt met Azure namens de gebruiker. Om in te stellen, start u met het toevoegen van Azure PowerShell bouwen eerst.

1. Selecteer **taak toevoegen**.
2. Zoeken naar **Azure PowerShell**. 
3. Als u deze vinden, selecteert u **toevoegen** de taak wordt toegevoegd aan de build. Als u dit doet, ziet u de taak als toegevoegd aan de linkerkant weergegeven.

![Stap PowerShell instellen](./media/set-up-devops-lab/set-up-powershell-step.png)

De snelste manier om het instellen van een service principal is om te laten Azure DevOps doen dat voor ons. 

1. Selecteer de **taak** u zojuist hebt toegevoegd.
2. Voor **Azure verbindingstype**, kiest u **Azure Resource Manager**. 
3. Selecteer de **beheren** koppeling voor het instellen van de service-principal. 

Zie voor meer informatie dit [blogbericht](https://devblogs.microsoft.com/devops/automating-azure-resource-group-deployment-using-a-service-principal-in-visual-studio-online-buildrelease-management/). Wanneer u selecteert de **beheren** koppeling, komt u in de juiste plaats in DevOps (tweede schermopname in het blogbericht) voor het instellen van de verbinding met Azure. Zorg ervoor dat u kiest **Azure Resource Manager-Service-eindpunt** wanneer u dit instelt.

## <a name="complete-the-build-task"></a>Voltooien van de build-taak
Als u de opbouwtaak selecteert, ziet u alle informatie in het rechterdeelvenster die moet worden ingevuld. 

1. Eerst, een naam voor de build-taak: **Maken van virtuele Machines**. 
2. Kies de **service-principal** u hebt gemaakt door te kiezen **Azure Resource Manager**
3. Kies de **service-eindpunt**. 
4. Voor **scriptpad**, selecteer **... (ellips)**  aan de rechterkant.
5. Navigeer naar **MakeGoldenImageVMs.ps1** script. 
6. Scriptparameters moeten er als volgt: `-ConfigurationLocation $(System.DefaultWorkingDirectory)$(ConfigurationLocation) -DevTestLabName $(DevTestLabName) -vmSize $(VMSize) -machineUserName $(MachineUserName) -machinePassword (ConvertTo-SecureString -string '$(MachinePassword)' -AsPlainText -Force) -StandardTimeoutMinutes $(StandardTimeoutMinutes)`

    ![Voltooien van de build-definitie](./media/set-up-devops-lab/complete-build-definition.png)


## <a name="queue-the-build"></a>De build-wachtrij
We controleren of u alles correct ingesteld door een nieuwe build queuing hebben. Terwijl de build wordt uitgevoerd, Ga naar de [Azure-portal](https://portal.azure.com) en selecteert u een op **alle virtuele Machines** in uw installatiekopie factory testomgeving om te controleren of alles goed werkt. U ziet drie virtuele machines in het lab gemaakt.

![Virtuele machines in het lab](./media/set-up-devops-lab/vms-in-lab.png)

## <a name="next-steps"></a>Volgende stappen 
De eerste stap bij het instellen van de installatiekopie van factory op basis van Azure DevTest Labs is voltooid. In het volgende artikel in de reeks krijgt u de virtuele machines gegeneraliseerd en opgeslagen in aangepaste installatiekopieën. Vervolgens hebben deze gedistribueerd naar uw labs. Zie het volgende artikel in de reeks: [Aangepaste installatiekopieën opslaan en distribueren naar meerdere labs](image-factory-save-distribute-custom-images.md).

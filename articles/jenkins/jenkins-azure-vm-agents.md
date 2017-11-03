---
title: Schalen Jenkins implementaties met agents van de virtuele machine van Azure.
description: Extra capaciteit toevoegt aan uw Jenkins pijplijnen met virtuele machines in Azure met de Jenkins Azure VM-Agent invoegtoepassing.
services: multiple
documentationcenter: 
author: rloutlaw
manager: justhe
ms.service: multiple
ms.workload: multiple
ms.topic: article
ms.date: 8/25/2017
ms.author: mlearned
ms.custom: Jenkins
ms.openlocfilehash: dbb30809ab68079666ecfa81a896c1d5101fb6fb
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2017
---
# <a name="scale-your-jenkins-deployments-to-meet-demand-with-azure-vm-agents"></a>Schalen van uw implementaties Jenkins om vraag te voldoen met Azure VM-agents

Deze zelfstudie laat zien hoe u de Jenkins [Azure VM Agents invoegtoepassing](https://plugins.jenkins.io/azure-vm-agents) capaciteit toe te voegen op aanvraag met Linux virtuele machines in Azure wordt uitgevoerd.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * De Azure VM Agents-invoegtoepassing installeren
> * Configureren van de invoegtoepassing voor het maken van resources in uw Azure-abonnement
> * De beschikbare rekenresources ingesteld op elke agent
> * Instellen van het besturingssysteem en de hulpprogramma's zijn geïnstalleerd op elke agent
> * Een nieuwe Jenkins freestyle taak maken
> * De taak uitgevoerd op een Azure VM-agent

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents/player]

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement
* Een Jenkins-hoofdserver. Als u niet hebt, raadpleegt u de [Quick Start](install-jenkins-solution-template.md) voor het instellen van een in Azure.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="install-azure-vm-agents-plugin"></a>Azure VM Agents-invoegtoepassing installeren

> [!TIP]
> Als u Jenkins geïmplementeerd op Azure worden verkregen met de [oplossingssjabloon](install-jenkins-solution-template.md), de Azure VM-Agent-invoegtoepassing is al geïnstalleerd.

1. Selecteer in het dashboard Jenkins **beheren Jenkins**, selecteer daarna **invoegtoepassingen beheren**.
2. Selecteer de **beschikbaar** tabblad en zoek naar **Azure VM Agents**. Schakel het selectievakje naast de vermelding voor de invoegtoepassing en selecteer **installeren zonder opnieuw opstarten** van de onderkant van het dashboard.

## <a name="configure-the-azure-vm-agents-plugin"></a>Configureren van de invoegtoepassing van Azure VM-Agents

1. Selecteer in het dashboard Jenkins **beheren Jenkins**, klikt u vervolgens **System configureren**.
2. Ga naar de onderkant van de pagina en zoek de **Cloud** sectie met de **toevoegen van nieuwe cloud** dropdown en kies **Microsoft Azure VM-Agents**.
3. Selecteer een bestaande service-principal van **toevoegen** omlaag in de **Azure-referenties** sectie. Als geen is vermeld, voert u de volgende stappen voor het [maken van een service-principal](/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager) voor uw Azure-account en toevoegen aan uw configuratie Jenkins:   

    a. Selecteer **toevoegen** naast **Azure-referenties** en kies **Jenkins**.   
    b. In de **toevoegen referenties** dialoogvenster Selecteer **Microsoft Azure Service-Principal** van de **soort** vervolgkeuzelijst.   
    c. Een Active Directory-Service-principal maken met Azure CLI of [Cloud Shell](/azure/cloud-shell/overview).
    
    ```azurecli-interactive
    az ad sp create-for-rbac --name jenkins_sp --password secure_password
    ```

    ```json
    {
        "appId": "BBBBBBBB-BBBB-BBBB-BBBB-BBBBBBBBBBB",
        "displayName": "jenkins_sp",
        "name": "http://jenkins_sp",
        "password": "secure_password",
        "tenant": "CCCCCCCC-CCCC-CCCC-CCCCCCCCCCC"
    }
    ```
    d. Voer de referenties van de service-principal in de **referenties toevoegen** dialoogvenster. Als u uw Azure-abonnement-ID niet weet, kunt u deze kunt opvragen met CLI:
     
     ```azurecli-interactive
     az account list
     ```

     ```json
        {
            "cloudName": "AzureCloud",
            "id": "AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA",
            "isDefault": true,
            "name": "Visual Studio Enterprise",
            "state": "Enabled",
            "tenantId": "CCCCCCCC-CCCC-CCCC-CCCC-CCCCCCCCCCC",
            "user": {
            "name": "raisa@fabrikam.com",
            "type": "user"
            }
     ```

    De voltooide service-principal moet gebruiken de `id` veld voor **abonnements-ID**, wordt de `appId` waarde voor **Client-ID**, `password` voor **Clientgeheim**, en een URL voor **OAuth 2.0-Tokeneindpunt** van `https://login.windows.net/<tenant_value>`. Selecteer **toevoegen** toevoegen van de service-principal en configureer vervolgens de invoegtoepassing voor het gebruik van de zojuist gemaakte referentie.

    ![Azure Service-Principal configureren](./media/jenkins-azure-vm-agents/new-service-principal.png)

    

4. In de **Resourcegroepnaam** sectie, laat u **nieuw** geselecteerd en voer `myJenkinsAgentGroup`.
5. Selecteer **controleren configuratie** verbinding maken met Azure voor het testen van de instellingen van het profiel.
6. Selecteer **toepassen** de configuratie van de invoegtoepassing bijwerken.

## <a name="configure-agent-resources"></a>Agent-resources configureren

Hiermee configureert u een sjabloon voor gebruik voor het definiëren van een Azure VM-agent. Deze sjabloon bepaalt de rekenresources elke agent wanneer is gemaakt.

1. Selecteer **toevoegen** naast **Azure virtuele Machine-sjabloon toevoegen**.
2. Voer `defaulttemplate` voor de **naam**
3. Voer `ubuntu` voor de **Label**
4. Selecteer de gewenste [Azure-regio](https://azure.microsoft.com/regions/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) van de keuzelijst met invoervak.
5. Selecteer een [VM-grootte](/azure/virtual-machines/linux/sizes) in de vervolgkeuzelijst onder **grootte van virtuele Machine**. Een algemeen `Standard_DS1_v2` grootte is voldoende voor deze zelfstudie.   
6. Laat de **bewaartijd** op `60`. Deze instelling bepaalt het aantal minuten die jenkins wachten kunt voordat het niet-actieve agents ongedaan. Geef 0 als u niet wilt dat niet-actieve agents worden automatisch verwijderd.

   ![Algemene VM-configuratie](./media/jenkins-azure-vm-agents/general-config.png)

## <a name="configure-agent-operating-system-and-tools"></a>Besturingssysteem van de agent en hulpprogramma's configureren

In de **imageconfiguratie** sectie van de invoegtoepassing configuratie, selecteert u **Ubuntu 16.04 LTS**. Schakel de selectievakjes naast **Git installeren (laatste)**, **installeren Maven (V3.5.0)**, en **installeren Docker** voor het installeren van deze hulpprogramma's op de zojuist gemaakte agents.

![Besturingssysteem van virtuele machine en hulpprogramma's configureren](./media/jenkins-azure-vm-agents/jenkins-os-config.png)

Selecteer **toevoegen** naast **beheerdersreferenties**, selecteer daarna **Jenkins**. Geef een gebruikersnaam en wachtwoord dat wordt gebruikt voor aanmelding bij de agents, alleen voor zorgen dat ze voldoen aan de [gebruikersnaam en wachtwoord beleid](/azure/virtual-machines/linux/faq#what-are-the-username-requirements-when-creating-a-vm) voor beheerdersaccounts op Azure Virtual machines.

Selecteer **sjabloon controleren** om te controleren of de configuratie en selecteer vervolgens **opslaan** uw wijzigingen niet opslaan en terugkeren naar het dashboard Jenkins.

## <a name="create-a-job-in-jenkins"></a>Een taak maken in Jenkins

1. Klik in het dashboard van Jenkins op **New Item**. 
2. Voer `demoproject1` voor de naam en selecteer **Freestyle project**, selecteer daarna **OK**.
3. In de **algemene** Kies **beperken waar project kan worden uitgevoerd** en het type `ubuntu` in **labelexpressie**. U ziet een bericht waarin wordt bevestigd dat het label wordt geleverd door de cloudconfiguratie in de vorige stap hebt gemaakt. 
   ![Taak instellen](./media/jenkins-azure-vm-agents/job-config.png)
4. In de **Source Code Management** tabblad **Git** en voeg de volgende URL in de **URL opslagplaats** veld:`https://github.com/spring-projects/spring-petclinic.git`
5. In de **bouwen** tabblad **toevoegen build stap**, klikt u vervolgens **aanroepen op het hoogste niveau Maven doelen**. Voer `package` in de **doelstellingen** veld.
6. Selecteer **opslaan** om op te slaan de taakdefinitie.

## <a name="build-the-new-job-on-an-azure-vm-agent"></a>De nieuwe taak maken op een Azure VM-agent

1. Ga terug naar het dashboard van Jenkins.
2. Selecteer de taak die u in de vorige stap hebt gemaakt en klik vervolgens op **nu samenstellen**. Een nieuwe build in de wachtrij staat, maar start niet totdat een VM-agent wordt gemaakt in uw Azure-abonnement.
3. Zodra de bewerking is voltooid, gaat u naar **Console Output**. U ziet dat de build op een Azure-agent op afstand is uitgevoerd.

![Console-uitvoer](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [CI/CD in Azure App Service](java-deploy-webapp-tutorial.md)

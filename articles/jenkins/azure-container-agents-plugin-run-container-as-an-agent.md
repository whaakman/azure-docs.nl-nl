---
title: Bouw een project in Azure met behulp van Jenkins en exemplaren van Azure-Container
description: Informatie over het gebruik van de Azure-Container Agent-invoegtoepassing voor Jenkins voor het bouwen van een project in Azure met Azure Containerexemplaren
services: multiple
documentationcenter: 
author: tomarcher
manager: rloutlaw
editor: 
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 12/11/2017
ms.author: tarcher
ms.custom: jenkins
ms.openlocfilehash: 04a60bf021ec6e265a3880264386ad32ec7e8177
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="build-a-project-in-azure-using-jenkins-and-azure-container-instances"></a>Bouw een project in Azure met behulp van Jenkins en exemplaren van Azure-Container

Azure Containerexemplaren maakt het gemakkelijk kunt laten en wordt uitgevoerd zonder te hoeven inrichten van virtuele machines of aannemen van een service op een hoger niveau. Azure Containerexemplaren biedt per seconde facturering op basis van de capaciteit die u nodig; maakt het een aantrekkelijke optie voor tijdelijke werkbelastingen zoals het uitvoeren van een build.

Procedures voor:
> [!div class="checklist"]
> * Installeren en configureren van een Jenkins-server op Azure
> * Installeren en configureren van de Azure-Container Agents-invoegtoepassing voor Jenkins
> * Exemplaren van Azure-Container gebruiken om de [Spring PetClinic voorbeeldtoepassing](https://github.com/spring-projects/spring-petclinic)

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement** - Raadpleeg voor meer over Azure-Aankoopopties [Azure aanschaffen](https://azure.microsoft.com/pricing/purchase-options/) of [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).

- **Azure CLI 2.0 of Azure Cloud Shell** -installeren van een van de volgende producten in voor het invoeren van de Azure-opdrachten:

    - [Azure CLI 2.0](/cli/azure/install-azure-cli?view=azure-cli-latest) -kunt u Azure-opdrachten uitvoeren vanuit een opdracht of een terminalvenster.
    - [Azure Cloud-Shell](/azure/cloud-shell/quickstart.md) - op Browser gebaseerde shell-ervaring. Cloud-Shell kan toegang tot een browser gebaseerde opdrachtregelprogramma ervaring gebouwd met Azure beheertaken in gedachten.

## <a name="install-a-jenkins-server-on-azure-using-the-jenkins-marketplace-image"></a>Een Jenkins-server op Azure met behulp van de Jenkins Marketplace-installatiekopie installeren

Jenkins ondersteuning biedt voor een model waarin de server gemachtigden op een of meer agents werken waarmee één Jenkins installatie voor het hosten van een groot aantal projecten of om verschillende omgevingen die nodig zijn voor Jenkins bouwt of test. De stappen in deze sectie helpt u bij het installeren en configureren van een Jenkins-server op Azure.

[!INCLUDE [jenkins-install-from-azure-marketplace-image](../../includes/jenkins-install-from-azure-marketplace-image.md)]

## <a name="connect-to-the-jenkins-server-running-on-azure"></a>Verbinding maken met de server Jenkins op Azure

Wanneer u Jenkins op Azure hebt geïnstalleerd, moet u verbinding maken met Jenkins. De volgende stappen maakt u door het instellen van een SSH-verbinding voor VM Jenkins uitgevoerd op Azure. 

[!INCLUDE [jenkins-connect-to-jenkins-server-running-on-azure](../../includes/jenkins-connect-to-jenkins-server-running-on-azure.md)]

## <a name="update-jenkins-dns"></a>Jenkins DNS bijwerken

Jenkins moet een eigen URL weet wanneer is het maken van koppelingen die naar zichzelf verwijst. De URL moet worden gebruikt wanneer Jenkins e-mailberichten met directe koppelingen verzonden voor het bouwen van resultaten. 

Deze sectie helpt u bij het instellen van de URL Jenkins.

1. Navigeer in uw browser naar het dashboard Jenkins op `http://localhost:8080`.

1. Selecteer **Jenkins beheren**.

    ![Opties in het dashboard Jenkins Jenkins beheren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Selecteer **systeem configureren**.

    ![Jenkins plugins optie in het dashboard Jenkins beheren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-system.png)

1. Onder **Jenkins locatie**, voer de URL van uw server Jenkins.

1. Selecteer **Opslaan**.

## <a name="update-jenkins-to-allow-java-network-launch-protocol-jnlp"></a>Update Jenkins waarmee Java netwerk starten Protocol (JNLP)

De agent Jenkins verbindt met de Jenkins-server via de Java netwerk starten Protocol (JNLP). Deze sectie wordt uitgelegd hoe u een poort voor JNLP agents moeten worden gebruikt bij het communiceren met de server Jenkins opgeven.

1. Selecteer in het dashboard Jenkins **Jenkins beheren**.

    ![Opties in het dashboard Jenkins Jenkins beheren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Selecteer **algemene beveiliging configureren**.

    ![Algemene beveiliging configureren in het dashboard Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-global-security.png)

1. Onder **Agents**, selecteer **vast**, en voert u een poort. De schermafbeelding toont een poortwaarde van 12345 als voorbeeld. U moet een poort opgeven die zinvol is voor uw omgeving.

    ![De globale beveiligingsinstellingen Jenkins zodat JNLP bijwerken](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-set-jnlp.png)

1. Selecteer **Opslaan**.

1. Met Azure CLI 2.0 of Cloud-Shell, voer de volgende opdracht voor het maken van een inkomende regel voor de netwerkbeveiligingsgroep voor uw Jenkins:

    ```azurecli
    az network nsg rule create  \
    --resource-group JenkinsResourceGroup \
    --nsg-name JenkinsNSG  \
    --name allow-https \
    --description "Allow access to port 12345 for HTTPS" \
    --access Allow \
    --protocol Tcp  \
    --direction Inbound \
    --priority 102 \
    --source-address-prefix "*"  \
    --source-port-range "*"  \
    --destination-address-prefix "*" \
    --destination-port-range "12345"
    ```

## <a name="create-and-add-an-azure-service-principal-to-the-jenkins-credentials"></a>Maken en toevoegen van een Azure-service-principal aan de Jenkins-referenties

U moet een Azure-service-principal te implementeren in Azure. De volgende stappen begeleiden u bij het proces van het maken van een service-principal (als u nog geen hebt) en Jenkins bijwerken met uw service-principal.

1. Als u al een service-principal (en bijbehorende abonnements-ID, tenant, toepassings-id en wachtwoord weet), kunt u deze stap overslaan. Als u een beveiligings-principal maken moet, raadpleegt u het artikel [een Azure-service-principal maken met Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest). Tijdens het maken van de principal Noteer de waarden voor de abonnements-ID, de tenant, de appId en het wachtwoord.

1. Selecteer **referenties**.

    ![De optie-referenties in het dashboard Jenkins beheren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials.png)

1. Onder **referenties**, selecteer **System**.

    ![Optie voor systeem-referenties in het dashboard Jenkins beheren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials-system.png)

1. Selecteer **globale referentie (onbeperkte)**.

    ![Globale systeem referenties optie in het dashboard Jenkins beheren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-credentials-global.png)

1. Selecteer **toe te voegen sommige referenties**.

    ![Referenties in het dashboard Jenkins toevoegen](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-adding-credentials.png)

1. In de **soort** vervolgkeuzelijst, selecteer **Microsoft Azure Service-Principal** leiden dat de pagina weer te geven velden die specifiek zijn voor het toevoegen van een service-principal. Klik, als volgt de aangevraagde waarden opgeeft:

    - **Bereik** -Selecteer de optie voor **Global (Jenkins, knooppunten, artikelen, alle onderliggende items, enz.)** .
    - **Abonnements-ID** -gebruik van de Azure-abonnement-ID die u hebt opgegeven bij het uitvoeren van `az account set`.
    - **Client-ID** -gebruik de `appId` geretourneerde waarde van `az ad sp create-for-rbac`.
    - **Clientgeheim** -gebruik de `password` geretourneerde waarde van `az ad sp create-for-rbac`.
    - **Tenant-ID** -gebruik de `tenant` geretourneerde waarde van `az ad sp create-for-rbac`.
    - **Azure-omgeving** : Selecteer `Azure`.
    - **ID** -Voer `myTestSp`. Deze waarde wordt verderop in deze zelfstudie gebruikt.
    - **Beschrijving** : (optioneel) Voer een beschrijvingswaarde voor deze principal.

    ![Geef nieuwe eigenschappen van de service-principal in het dashboard Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-principal-properties.png)

1. Wanneer u de informatie voor het definiëren van de principal hebt ingevoerd, kunt u eventueel selecteren **Controleer of Service-Principal** om ervoor te zorgen dat alles goed werkt. Als uw service-principal correct is gedefinieerd, ziet u een bericht weergegeven 'geverifieerd dat de Service-Principal Microsoft Azure." onder de **beschrijving** veld.

1. Wanneer u klaar bent, selecteert u **OK** toevoegen van de principal aan Jenkins. Het dashboard Jenkins geeft de zojuist toegevoegde principal weer op de **globale referentie** pagina.

## <a name="create-an-azure-resource-group-for-your-azure-container-instances"></a>Een Azure-resourcegroep maken voor uw Azure-Container-exemplaren

Azure Container-exemplaren moet worden geplaatst in een Azure-resourcegroep. Een Azure-resourcegroep is een container met verwante resources voor een Azure-oplossing.

Met behulp van Azure CLI 2.0 of Cloud-Shell, voer de volgende opdracht voor het maken van een resourcegroep aangeroepen `JenkinsAciResourceGroup` op locatie `eastus`:

```azurecli
az group create --name JenkinsAciResourceGroup --location eastus
```

Als u klaar bent de `az group create` opdracht resultaten vergelijkbaar met het volgende voorbeeld wordt weergegeven:

```JSON
{
  "id": "/subscriptions/<subscriptionId>/resourceGroups/JenkinsAciResourceGroup",
  "location": "eastus",
  "managedBy": null,
  "name": "JenkinsAciResourceGroup",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "tags": null
}
```

## <a name="install-the-azure-container-agents-plugin-for-jenkins"></a>De Azure-Container Agents-invoegtoepassing voor Jenkins installeren

Als u de invoegtoepassing van Azure Container Agents al hebt geïnstalleerd, kunt u deze sectie overslaan.

Zodra u de Azure-resourcegroep gemaakt voor de agent Jenkins hebt, ziet u de volgende stappen uit het installeren van de invoegtoepassing van Azure Container Agents:

1. Selecteer in het dashboard Jenkins **Jenkins beheren**.

    ![Opties in het dashboard Jenkins Jenkins beheren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Selecteer **invoegtoepassingen beheren**.

    ![Jenkins plugins optie in het dashboard Jenkins beheren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-plugins.png)

1. Selecteer **beschikbaar**.

    ![Beschikbare Jenkins plugins optie in het dashboard Jenkins weergeven](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-view-available-plugins.png)

1. Voer `Azure Container Agents` in de **Filter** in het tekstvak. (De lijst met filters voor als u de tekst invoeren).

    ![De beschikbare Jenkins invoegtoepassingen in het dashboard Jenkins filteren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-filter-available-plugins.png)

1. Schakel het selectievakje in naast de **Azure Container Agents** invoegtoepassing, en een van de installatieopties. Voor de doeleinden van deze demo ik hebt geselecteerd de **installeren zonder opnieuw opstarten** optie.

    ![De invoegtoepassingen van Azure Container Agents installeren vanuit het dashboard Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-install-aks-agent-plugin.png)

1.  Nadat u de optie voor het installeren van de gewenste plugin(s) selecteert, geeft het dashboard Jenkins weer een pagina met gedetailleerde informatie over de status van wat u installeert.

    ![Status van de installatie van de installatie van de Azure-Container Agents plugins vanaf het dashboard Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-install-aks-agent-plugin-confirmation.png)

    Selecteer voor de hoofdpagina van het dashboard Jenkins, **terug te keren naar de eerste pagina**.

## <a name="configure-the-azure-container-agents-plugin"></a>Configureren van de invoegtoepassing van Azure Container Agents

Nadat de Azure-Container Agents-invoegtoepassing is geïnstalleerd, helpt in deze sectie u de invoegtoepassing in het dashboard Jenkins configureren.

1. Selecteer in het dashboard Jenkins **Jenkins beheren**.

    ![Opties in het dashboard Jenkins Jenkins beheren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-manage-jenkins.png)

1. Selecteer **systeem configureren**.

    ![Jenkins plugins optie in het dashboard Jenkins beheren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-configure-system.png)

1. Zoek de **Cloud** sectie aan de onderkant van de pagina en van de **toevoegen van een nieuwe cloud** vervolgkeuzelijst, selecteer **exemplaar van Azure-Container**.

    ![Een nieuwe cloudprovider van de vanuit het dashboard Jenkins toevoegen](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-cloud-provider.png)

1. In de **Azure Container exemplaar** sectie, geeft u de volgende waarden:

    - **Naam van cloud** : (optioneel als deze waarde wordt standaard ingesteld op een automatisch gegenereerde naam.) Geef een naam voor dit exemplaar. 
    - **Azure referentie** : Selecteer de pijl en selecteer vervolgens de `myTestSp` vermelding die de Azure service-principal identificeert u eerder hebt gemaakt.
    - **Resourcegroep** : Selecteer de pijl en selecteer vervolgens de `JenkinsAciResourceGroup` vermelding die de instantie van de Container Azure-resourcegroep identificeert u eerder hebt gemaakt.

    ![Eigenschappen van de Azure-Container instantie definiëren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-properties.png)

1. Selecteer de **Container sjabloon toevoegen** vervolgkeuzepijl, en selecteer vervolgens **Aci Container sjabloon**.

1. In de **Aci container sjabloon** sectie, geeft u de volgende waarden:

    - **Naam** -Voer `ACI-container`.
    - **Labels** -Voer `ACI-container`.
    - **Afbeelding van docker** -invoeren`cloudbees/jnlp-slave-with-java-build-tools`

    ![De eigenschappen van de installatiekopie van het exemplaar van Azure Container definiëren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-image-properties.png)

1. Selecteer **geavanceerde**.

1. Selecteer de **bewaren strategie** vervolgkeuzepijl, en selecteer **Container niet-actieve bewaren strategie**. Als u deze optie selecteert, houdt Jenkins u de agent totdat er geen nieuwe taak wordt uitgevoerd op de agent en de opgegeven niet-actieve tijd is verstreken.

    ![De strategie voor de bewaarperiode Azure Container exemplaar definiëren](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-aci-retention-strategy.png)

1. Selecteer **Opslaan**.

## <a name="create-the-spring-petclinic-application-job-in-jenkins"></a>De taak Spring PetClinic toepassing maken in Jenkins

De volgende stappen helpen u bij het maken van een taak Jenkins - als een freestyle project - de toepassing van de PetClinic Spring te bouwen.

1. Selecteer in het dashboard Jenkins **Nieuw Item**.

    ![Menuoptie Nieuw item in het dashboard Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-item.png)

1. Voer `myPetClinicProject` voor de itemnaam en selecteer **Freestyle project**.

    ![Nieuw freestyle project in het dashboard Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-freestyle-project.png)

1. Selecteer **OK**.

1. Selecteer de **algemene** tabblad en geef de volgende waarden:

    - **Beperken waar project kan worden uitgevoerd** -Selecteer deze optie.
    - **Expressie labelen** -Voer `ACI-container`. Als u het veld verlaat, verschijnt een bericht waarin wordt bevestigd dat het label wordt geleverd door de cloudconfiguratie in de vorige stap hebt gemaakt.

    ![Algemene instellingen voor een nieuw freestyle project in het dashboard Jenkins](./media/azure-container-agents-plugin-run-container-as-an-agent/jenkins-dashboard-new-freestyle-project-general.png)

1. Selecteer de **beheer van gegevensbronnen Code** tabblad en geef de volgende waarden:

    - **Bron Code Management** : Selecteer **Git**.
    - **URL van de opslagplaats** -Voer de volgende URL voor de versie die voorjaar PetClinic voorbeeld toepassing GitHub-repo: `https://github.com/spring-projects/spring-petclinic.git`.

1. Selecteer de **bouwen** tabblad en de volgende taken uitvoeren:

    - Selecteer de **toevoegen build stap** vervolgkeuzepijl, en selecteer **aanroepen op het hoogste niveau Maven doelen**.

    - Voor **doelstellingen**, voer `package`.

1. Selecteer **opslaan** om op te slaan van het nieuwe projectdefinitie.

## <a name="build-the-spring-petclinic-application-job-in-jenkins"></a>De taak Spring PetClinic toepassing in Jenkins bouwen

Het is nu tijd om uw project te bouwen. Deze sectie wordt uitgelegd hoe u een project vanuit het dashboard Jenkins maakt.

1. Selecteer in het dashboard Jenkins `myPetClinicProject`.

    ![Selecteer het project bouwen vanuit het dashboard Jenkins.](./media/azure-container-agents-plugin-run-container-as-an-agent/select-project-to-build.png)

1. Selecteer **nu samenstellen**. 

    ![Bouw het project vanuit het dashboard Jenkins.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-project.png)

1. Wanneer u een build in Jenkins start, wordt de build in de wachtrij geplaatst. In het geval van een Azure-Container-Agent, kan niet de build worden uitgevoerd totdat de Azure-Container-Agent is gestart en online worden gebracht. Tot er een bericht met de Agentnaam van de en het feit dat de build in behandeling is. (Dit proces duurt ongeveer vijf minuten, maar hoeft alleen de eerste keer dat u de agent voor een build gebruiken. Volgende versies zijn veel sneller als de agent op dat moment online is.)

    ![De build in de wachtrij staat totdat de agent is gemaakt en online worden gebracht.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-pending.png)

    Zodra de build start, een voortgangsbalk barber-pool wordt aangegeven dat de build is uitgevoerd:

    ![De build wordt uitgevoerd, eenmaal u de voortgangsbalk barber-pool ziet.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-running.png)

1. Als de voortgangsbalk barber pool verdwijnt, selecteert u de pijl naast het build-nummer. Selecteer in het contextmenu **Console uitvoer**.

    ![Klik op de menuopdracht Consolelogboek om de informatie weer te geven.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log-menu.png)

1. De console logboekgegevens uit het buildproces wordt verzonden. Selecteer om te zien alle build-gegevens (inclusief informatie over de build op afstand wordt uitgevoerd op de Azure-agent die u hebt gemaakt) **volledig logboek**.

    ![Klik op de koppeling volledig logboek om meer gedetailleerde build-informatie weer te geven.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log.png)

    Het volledige logboek weergegeven uitgebreidere build informatie:

    ![Het volledige logboek weergeven uitgebreidere build-informatie](./media/azure-container-agents-plugin-run-container-as-an-agent/build-console-log-full.png)
    
1. Als u wilt weergeven van de build toestand, Ga naar de onderkant van het logboek.

    ![De toestand van de build wordt weergegeven onder aan het build-logboek.](./media/azure-container-agents-plugin-run-container-as-an-agent/build-disposition.png)

## <a name="clean-up-azure-resources"></a>Azure-resources opschonen

In deze zelfstudie maakt u bronnen die zich bevinden binnen twee Azure-resourcegroepen gemaakt: 
    - `JenkinsResourceGroup`-Bevat de Azure-resources voor de Jenkins-server.
    - `JenkinsAciResourceGroup`-De Azure-resources voor de agent Jenkins bevat.
    
Als u niet langer gebruiken een van de resources in een Azure-resourcegroep wilt, kunt u verwijderen resource groep met de `az group delete` opdracht als volgt (vervangt de &lt;resourceGroup > aanduiding voor items met de naam van de resourcegroep die u wilt verwijderen):

```azurecli
az group delete -n <resourceGroup>
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Ga naar de Jenkins op Azure hub om te zien van de meest recente artikelen en voorbeelden](https://docs.microsoft.com/azure/jenkins/)
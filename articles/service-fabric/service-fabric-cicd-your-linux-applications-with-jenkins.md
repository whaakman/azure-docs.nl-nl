---
title: Doorlopend bouwen en integreren voor uw Azure Service Fabric Linux-toepassingen met behulp van Jenkins | Microsoft Docs
description: Doorlopend bouwen en integreren voor uw Service Fabric Linux-toepassing met behulp van Jenkins
services: service-fabric
documentationcenter: java
author: sayantancs
manager: jpconnock
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/31/2018
ms.author: saysa
ms.openlocfilehash: 7abc15264a44c969f57071e84ffcedca30d326fb
ms.sourcegitcommit: 415742227ba5c3b089f7909aa16e0d8d5418f7fd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55766313"
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-applications"></a>Jenkins gebruiken om te bouwen en implementeren van uw Linux-toepassingen
Jenkins is een populair hulpprogramma voor doorlopende integratie en implementatie van uw apps. Hier leest u hoe u een Azure Service Fabric-toepassing maakt en implementeert met behulp van Jenkins.

## <a name="topic-overview"></a>Onderwerp-overzicht
In dit artikel bevat informatie over verschillende manieren van het instellen van uw Jenkins-omgeving en de verschillende manieren voor het implementeren van uw toepassing in een Service Fabric-cluster nadat deze is gemaakt. Volg deze algemene stappen voor is Jenkins instellen, pull-wijzigingen vanuit GitHub, uw toepassing bouwen en deze implementeren in uw cluster:

1. Zorg ervoor dat u installeert de [vereisten](#prerequisites).
1. Volg de stappen in een van deze secties voor het instellen van Jenkins op:
   * [Jenkins instellen in een Service Fabric-cluster](#set-up-jenkins-inside-a-service-fabric-cluster), 
   * [Jenkins instellen buiten een Service Fabric-cluster](#set-up-jenkins-outside-a-service-fabric-cluster), of
   * [De Service Fabric-invoegtoepassing installeren in een bestaande Jenkins-omgeving](#install-service-fabric-plugin-in-an-existing-jenkins-environment).
1. Wanneer u Jenkins hebt ingesteld, volgt u de stappen in [maken en configureren van een Jenkins-taak](#create-and-configure-a-jenkins-job) voor het instellen van GitHub trigger Jenkins wanneer wijzigingen worden aangebracht in uw toepassing en het configureren van uw Jenkins-taak pijplijn via de build-stap voor het ophalen van de verandert van GitHub en uw toepassing te bouwen. 
1. Ten slotte, configureert u de Jenkins-taak na build-stap om uw toepassing in uw Service Fabric-cluster te implementeren. Er zijn twee manieren om Jenkins voor het implementeren van uw toepassing in een cluster te configureren:    
   * Gebruik voor ontwikkel- en testomgevingen, [configureren van de implementatie met behulp van de cluster-beheereindpunt](#configure-deployment-using-cluster-management-endpoint). Dit is de eenvoudigste implementatiemethode om in te stellen.
   * Gebruik voor productie-omgevingen, [implementatie met behulp van Azure-referenties configureren](#configure-deployment-using-azure-credentials). Deze methode voor productieomgevingen wordt aangeraden omdat met de Azure-referenties kunt u de toegang van een Jenkins-taak beperken tot uw Azure-resources. 

## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat Git lokaal is geïnstalleerd. U kunt de juiste Git-versie installeren [de Git-downloadpagina](https://git-scm.com/downloads) op basis van uw besturingssysteem. Als u bekend bent met Git bent, meer informatie vindt u op de [Git-documentatie](https://git-scm.com/docs).
- In dit artikel wordt de *Service Fabric ophalen van de slag-voorbeeld* op GitHub: [ https://github.com/Azure-Samples/service-fabric-java-getting-started ](https://github.com/Azure-Samples/service-fabric-java-getting-started) voor de toepassing te bouwen en implementeren. U kunt deze opslagplaats om mee te splitsen, of, met enkele aanpassingen aan de instructies, uw eigen GitHub-project.


## <a name="install-service-fabric-plugin-in-an-existing-jenkins-environment"></a>Service Fabric-invoegtoepassing installeren in een bestaande Jenkins-omgeving
Als u de Service Fabric-invoegtoepassing aan een bestaande Jenkins-omgeving toevoegt, moet u het volgende:

- De [Service Fabric-CLI](service-fabric-cli.md) (sfctl).

   > [!NOTE]
   > Zorg ervoor dat de CLI op het systeemniveau van het installeren in plaats van op het gebruikersniveau van de, dus Jenkins kunt CLI-opdrachten uitvoeren. 
   >

- Voor het implementeren van Java-toepassingen installeren [Gradle en Open JDK 8.0](service-fabric-get-started-linux.md#set-up-java-development). 
- .NET Core 2.0 om toepassingen te implementeren, installeert de [.NET Core 2.0 SDK](service-fabric-get-started-linux.md#set-up-net-core-20-development). 

Nadat u de vereisten die nodig zijn voor uw omgeving hebt geïnstalleerd, kunt u zoeken naar de Azure Service Fabric-invoegtoepassing in Jenkins marketplace en installeer deze.

Nadat u de invoegtoepassing hebt geïnstalleerd, gaat u verder met [maken en configureren van een Jenkins-taak](#create-and-configure-a-jenkins-job).


## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Jenkins instellen in een Service Fabric-cluster

U kunt Jenkins instellen binnen of buiten een Service Fabric-cluster. De volgende secties laten zien hoe deze instellen in een cluster tijdens het gebruik van een Azure storage-account om op te slaan de status van de containerinstantie.

### <a name="prerequisites"></a>Vereisten
- Een Linux Service Fabric-cluster hebt met Docker is geïnstalleerd. Service Fabric-clusters die al worden uitgevoerd in Azure zijn Docker geïnstalleerd. Als u lokaal het cluster (OneBox-ontwikkelomgeving) uitvoert, controleert u of Docker is geïnstalleerd op uw computer met de `docker info` opdracht. Als dit niet is geïnstalleerd, installeert u deze met behulp van de volgende opdrachten:

   ```sh
   sudo apt-get install wget
   wget -qO- https://get.docker.io/ | sh
   ``` 

   > [!NOTE]
   > Zorg ervoor dat de poort 8081 is opgegeven als een aangepast eindpunt op het cluster. Als u een lokaal cluster gebruikt, zorg ervoor dat poort 8081 geopend op de hostcomputer is en dat er een openbare IP-adres.
   >

### <a name="steps"></a>Stappen
1. Kloon de toepassing met behulp van de volgende opdrachten:
   ```sh
   git clone https://github.com/suhuruli/jenkins-container-application.git
   cd jenkins-container-application
   ```

1. De status van de Jenkins-container in een bestandsshare behouden:
   1. Maak een Azure storage-account in de **dezelfde regio** als uw cluster met een naam, zoals `sfjenkinsstorage1`.
   1. Maak een **bestandsshare** onder de storage-Account met een naam, zoals `sfjenkins`.
   1. Klik op **Connect** voor de bestandsshare en noteer de waarden wordt weergegeven onder **verbinding maken vanaf Linux**, de waarde moet er ongeveer als volgt uit:

      ```sh
      sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
      ```

   > [!NOTE]
   > Om te koppelen, cifs shares moet u beschikken over de cifs-utils-pakket geïnstalleerd op de clusterknooppunten.      
   >

1. Bijwerken van de tijdelijke aanduiding voor waarden in de `setupentrypoint.sh` script met de details van de azure-opslag van stap 2.
   ```sh
   vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
   ```
   * Vervang `[REMOTE_FILE_SHARE_LOCATION]` met de waarde `//sfjenkinsstorage1.file.core.windows.net/sfjenkins` uit de uitvoer van de verbinding te maken in stap 2 hierboven.
   * Vervang `[FILE_SHARE_CONNECT_OPTIONS_STRING]` met de waarde `vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777` uit stap 2 hierboven.

1. **Alleen beveiligd Cluster:** 
   
   Voor het configureren van de implementatie van toepassingen op een beveiligde cluster op basis van Jenkins, moet het clustercertificaat toegankelijk zijn in de Jenkins-container. In de *ApplicationManifest.xml* bestand, onder de **ContainerHostPolicies** tag deze certificaatverwijzing toevoegen en bijwerken van de vingerafdrukwaarde met die van het clustercertificaat.

   ```xml
   <CertificateRef Name="MyCert" X509FindValue="[Thumbprint]"/>
   ```

   Voeg bovendien de volgende regels onder de **ApplicationManifest** (root)-code in de *ApplicationManifest.xml* -bestand en werk de vingerafdrukwaarde met die van het clustercertificaat.

   ```xml
   <Certificates>
     <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="[Thumbprint]" />
   </Certificates> 
   ```

1. Verbinding maken met het cluster en de containertoepassing installeren.

   **Cluster beveiligen**
   ```sh
   sfctl cluster select --endpoint https://PublicIPorFQDN:19080  --pem [Pem] --no-verify # cluster connect command
   bash Scripts/install.sh
   ```
   De voorgaande opdracht wordt het certificaat in de PEM-indeling. Als uw certificaat in PFX-indeling is, kunt u de volgende opdracht uit om deze te converteren. Als uw PFX-bestand niet is beveiligd met een wachtwoord, geeft u de **passin** parameter als `-passin pass:`.
   ```sh
   openssl pkcs12 -in cert.pfx -out cert.pem -nodes -passin pass:MyPassword1234!
   ``` 
   
   **Niet-beveiligd Cluster**
   ```sh
   sfctl cluster select --endpoint http://PublicIPorFQDN:19080 # cluster connect command
   bash Scripts/install.sh
   ```

   Hiermee wordt een Jenkins-container in het cluster geïnstalleerd. Dit kan worden bewaakt met de Service Fabric Explorer.

   > [!NOTE]
   > Het duurt enkele minuten voor de Jenkins-installatiekopie moet worden gedownload op het cluster.
   >

1. Ga in uw browser naar `http://PublicIPorFQDN:8081`. Hier vindt u het pad naar het eerste beheerderswachtwoord dat nodig is om u aan te melden. 
1. Bekijk de Service Fabric Explorer om te bepalen op welk knooppunt de Jenkins-container wordt uitgevoerd. Secure Shell (SSH) aanmelden bij dit knooppunt.
   ```sh
   ssh user@PublicIPorFQDN -p [port]
   ``` 
1. Haal de exemplaar-id van de container op met `docker ps -a`.
1. Secure Shell (SSH) aanmelden bij de container en plak het pad dat u is getoond in de Jenkins-portal. Bijvoorbeeld, als in de portal wordt het pad `PATH_TO_INITIAL_ADMIN_PASSWORD`, voer de volgende opdrachten uit:

   ```sh
   docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
   ```
   ```sh
   cat PATH_TO_INITIAL_ADMIN_PASSWORD # This displays the password value
   ```
1. Kies op de pagina Jenkins aan de slag, de optie invoegtoepassingen voor de installatie-optie, selecteert u de **geen** selectievakje en klik op installeren.
1. Een gebruiker maken of selecteren om door te gaan als een beheerder.

Nadat u Jenkins hebt ingesteld, gaat u verder met [maken en configureren van een Jenkins-taak](#create-and-configure-a-jenkins-job).  

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Jenkins instellen buiten een Service Fabric-cluster

U kunt Jenkins instellen binnen of buiten een Service Fabric-cluster. In de volgende secties ziet u hoe u Jenkins instelt buiten een cluster.

### <a name="prerequisites"></a>Vereisten
- Zorg ervoor dat Docker is geïnstalleerd op uw computer. De volgende opdrachten kunnen worden gebruikt om Docker vanuit de terminal te installeren:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

  Bij het uitvoeren van `docker info` in de terminal, de uitvoer moet worden weergegeven dat de Docker-service wordt uitgevoerd.

### <a name="steps"></a>Stappen
1. Haal de Service Fabric Jenkins container-installatiekopie op: `docker pull rapatchi/jenkins:latest`. Deze installatiekopie wordt geleverd met de Service Fabric Jenkins-invoegtoepassing die vooraf is geïnstalleerd.
1. Voer de installatiekopie van de container uit: `docker run -itd -p 8080:8080 rapatchi/jenkins:latest`
1. Haal de id op van het exemplaar van de installatiekopie van de container. U kunt een lijst van alle Docker-containers bekijken met de opdracht `docker ps –a`
1. Meld u aan bij de Jenkins-portal met de volgende stappen uit:

   1. Meld u op uw host bij een Jenkins-shell. Gebruik de eerste vier cijfers van de container-ID. Bijvoorbeeld, als de container-ID is `2d24a73b5964`, gebruikt u `2d24`.

      ```sh
      docker exec -it [first-four-digits-of-container-ID] /bin/bash
      ```
   1. Haal het beheerderswachtwoord voor uw container instance van de Jenkins-shell:

      ```sh
      cat /var/jenkins_home/secrets/initialAdminPassword
      ```      
   1. Als u wilt aanmelden bij de Jenkins-dashboard, opent u de volgende URL in een webbrowser: `http://<HOST-IP>:8080`. Gebruik het wachtwoord van de vorige stap voor het ontgrendelen van Jenkins.
   1. (Optioneel.) Nadat u zich hebt aangemeld voor de eerste keer, u kunt uw eigen gebruikersaccount maken en gebruiken die voor de volgende stappen uit of u kunt echter ook doorgaan met het administrator-account. Als u een gebruiker maakt, moet u om door te gaan met die gebruiker.
1. Stel GitHub in voor Jenkins met behulp van de stappen in [een nieuwe SSH-sleutel genereren en toe te voegen aan de SSH-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
   * Volg de instructies van GitHub om de SSH-sleutel te genereren en deze toe te voegen aan het GitHub-account waarop de opslagplaats wordt gehost.
   * Voer de opdrachten die in de vorige koppeling zijn genoemd, uit in de Jenkins Docker-shell (en niet op uw host).
   * Gebruik de volgende opdracht om u vanaf uw host aan te melden bij de Jenkins-shell:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Zorg ervoor dat het cluster of de computer waar de Jenkins-containerinstallatiekopie wordt gehost een openbare IP-adres heeft. Alleen zo kan het Jenkins-exemplaar meldingen ontvangen van GitHub.

Nadat u Jenkins hebt ingesteld, Ga verder met de volgende sectie, [maken en configureren van een Jenkins-taak](#create-and-configure-a-jenkins-job).

## <a name="create-and-configure-a-jenkins-job"></a>Een Jenkins-taak maken en configureren

De stappen in deze sectie laten zien hoe u een Jenkins-taak om te reageren op wijzigingen in een GitHub-opslagplaats, worden de wijzigingen opgehaald en bouwt u ze configureren. Aan het einde van deze sectie, bent u omgeleid naar de laatste stappen voor het configureren van de taak voor het implementeren van uw toepassing op basis van of u een omgeving voor ontwikkelen/testen of in een productieomgeving implementeert. 

1. Klik op het dashboard van Jenkins **Nieuw Item**.
1. Voer een itemnaam in (bijvoorbeeld **MyJob**). Selecteer **free-style project** en klik op **OK**.
1. De configuratiepagina van de taak wordt geopend. (Als u de configuratie in de Jenkins-dashboard, klikt u op de taak en klik vervolgens op **configureren**).

1. Op de **algemene** tabblad, schakel het selectievakje voor **GitHub-project**, en geef de URL van uw GitHub-project. Dit is de URL waarop de Service Fabric Java-toepassing wordt gehost die u wilt integreren met de CI-/CD-stroom van Jenkins (Continue integratie, Continue implementatie), bijvoorbeeld `https://github.com/{your-github-account}/service-fabric-java-getting-started`.

1. Op de **Source Code Management** tabblad **Git**. Geef de opslagplaats-URL op waarop de Service Fabric Java-toepassing wordt gehost die u wilt integreren met de CI-/CD-stroom van Jenkins, bijvoorbeeld `https://github.com/{your-github-account}/service-fabric-java-getting-started`. U kunt ook opgeven welke vertakking u wilt maken (bijvoorbeeld `/master`).
1. Configureer uw *GitHub* opslagplaats om te communiceren met Jenkins:

   a. Ga op de pagina van uw GitHub-opslagplaats naar **instellingen** > **integraties en Services**.

   b. Selecteer **Add Service**, typ **Jenkins** en selecteer de **Jenkins-GitHub-invoegtoepassing**.

   c. Geef de Jenkins-webhook-URL op (standaard is dit `http://<PublicIPorFQDN>:8081/github-webhook/`). Klik op **add/update service**.

   d. Er wordt een testgebeurtenis verzonden naar uw Jenkins-exemplaar. Er komt een groen vinkje bij de webhook te staan in GitHub en uw project wordt gemaakt.

1. Op de **Triggers bouwen** tabblad in Jenkins, selecteert u welke optie u wilt. In dit voorbeeld die u wilt een build activeren wanneer er een push naar de opslagplaats, dus selecteer **GitHub-hookactivatie voor GITScm-polling**. (Voorheen heette deze optie **Build when a change is pushed to GitHub**.)
1. Op de **bouwen** tabblad, doe het volgende, afhankelijk van of u een Java-toepassing of een .NET Core-App bouwt:

   * **Voor Java-toepassingen:** Uit de **build-stap toevoegen** vervolgkeuzelijst, selecteer **Invoke Gradle Script**. Klik op **Advanced**. Geef het pad naar in het menu Geavanceerd **hoofdscript** voor uw toepassing. Het haalt build.gradle op uit het opgegeven pad en werkt dienovereenkomstig. Voor de [ActorCounter toepassing](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/reliable-services-actor-sample/Actors/ActorCounter), dit is: `${WORKSPACE}/reliable-services-actor-sample/Actors/ActorCounter`.

     ![Service Fabric Jenkins Build-actie][build-step]

   * **Voor .NET Core-toepassingen:** Uit de **build-stap toevoegen** vervolgkeuzelijst, selecteer **Shell uitvoeren**. In het vak in de opdracht die wordt weergegeven, moet de map eerst moet worden gewijzigd in het pad waar het bestand build.sh zich bevindt. Zodra de map is gewijzigd, wordt het script build.sh kan worden uitgevoerd en wordt de toepassing gebouwd.

      ```sh
      cd /var/jenkins_home/workspace/[Job Name]/[Path to build.sh]  # change directory to location of build.sh file
      ./build.sh
      ```

     De volgende schermafbeelding ziet u een voorbeeld van de opdrachten die worden gebruikt om te bouwen de [teller Service](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started/tree/master/Services/CounterService) voorbeeld met de naam van een Jenkins-taak van CounterServiceApplication.

      ![Service Fabric Jenkins Build-actie][build-step-dotnet]

1. Voor het configureren van Jenkins voor het implementeren van uw app in een Service Fabric-cluster in de acties na bouwen, moet u de locatie van het certificaat van het cluster in uw Jenkins-container. Kies een van de volgende, afhankelijk van of uw Jenkins-container wordt uitgevoerd binnen of buiten uw cluster en noteer de locatie van het clustercertificaat:

   * **Voor Jenkins die binnen het cluster wordt uitgevoerd:** Het pad naar het certificaat kan worden gevonden door de waarde van deze echo de *Certificates_JenkinsOnSF_Code_MyCert_PEM* omgevingsvariabele uit in de container.

      ```sh
      echo $Certificates_JenkinsOnSF_Code_MyCert_PEM
      ```
   
   * **Voor Jenkins uitgevoerd buiten het cluster:** Volg deze stappen voor het clustercertificaat kopiëren naar de container:
      1. Het certificaat moet zich in de PEM-indeling. Als u geen een PEM-bestand hebt, kunt u een van de certificaat-PFX-bestand kunt maken. Als uw PFX-bestand niet beveiligd met een wachtwoord is, voert u de volgende opdracht uit vanaf uw host:

         ```sh
         openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:
         ``` 

      Als het PFX-bestand beveiligd met een wachtwoord is, voegt u het wachtwoord in de `-passin` parameter. Bijvoorbeeld:

         ```sh
         openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:MyPassword1234!
         ``` 

      1. Als u de container-ID voor uw Jenkins-container, uitgevoerd `docker ps` vanaf uw host.
      1. Kopieer het PEM-bestand naar de container met de volgende Docker-opdracht:
    
         ```sh
         docker cp clustercert.pem [first-four-digits-of-container-ID]:/var/jenkins_home
         ``` 

U bent bijna klaar! Houd de Jenkins-taak geopend. De enige resterende taak bestaat uit het configureren van de stappen na bouwen voor het implementeren van uw toepassing in uw Service Fabric-cluster:

* Als u wilt implementeren in een omgeving voor ontwikkeling of tests, volg de stappen in [configureren van de implementatie met behulp van de cluster-beheereindpunt](#configure-deployment-using-cluster-management-endpoint).
* Als u wilt implementeren in een productieomgeving, volg de stappen in [implementatie met behulp van Azure-referenties configureren](#configure-deployment-using-azure-credentials).

## <a name="configure-deployment-using-cluster-management-endpoint"></a>Implementatie met behulp van de cluster-beheereindpunt configureren
U kunt het eindpunt voor Clusterbeheer voor ontwikkel- en testomgevingen gebruiken om uw toepassing te implementeren. Configureren van de actie na bouwen met het eindpunt voor Clusterbeheer om uw toepassing te implementeren, moet instellen zo min mogelijk. Als u in een productieomgeving implementeert, verder naar [implementatie met behulp van Azure-referenties configureren](#configure-deployment-using-azure-credentials) het configureren van een service-principal voor Azure Active Directory om te gebruiken tijdens de implementatie.    

1. Klik in de Jenkins-taak op de **acties na bouwen** tabblad. 
1. Selecteer in de vervolgkeuzelijst **Post-Build Actions** de optie **Post-Build Actions**. 
1. Onder **configuratie van Service Fabric-Cluster**, selecteer de **vult u de Service Fabric-Beheereindpunt** keuzerondje.
1. Voor **Management Host**, voer het verbindingseindpunt voor uw cluster; bijvoorbeeld `{your-cluster}.eastus.cloudapp.azure.com`.
1. Voor **Clientsleutel** en **clientcertificaat**, geef de locatie van het PEM-bestand in uw Jenkins-container; bijvoorbeeld `/var/jenkins_home/clustercert.pem`. (U hebt de laatste stap van de locatie van het certificaat gekopieerd [maken en configureren van een Jenkins-taak](#create-and-configure-a-jenkins-job).)
1. Onder **Toepassingsconfiguratie**, configureer de **toepassingsnaam**, **toepassingstype**, en de (relatief) **pad naar het Manifest van de toepassing** velden.

   ![Service Fabric Jenkins na Build-actie configureren beheereindpunt](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-endpoint.png)

1. Klik op **configuratie controleren**. Klik op een geslaagde verificatie **opslaan**. De pijplijn van uw Jenkins-taak is nu volledig geconfigureerd. Gaat u verder met [Vervolgstappen](#next-steps) om uw implementatie te testen.

## <a name="configure-deployment-using-azure-credentials"></a>Implementatie met behulp van Azure-referenties configureren
Voor productieomgevingen, wordt configureren van een Azure-referentie voor het implementeren van uw toepassing sterk aanbevolen. In deze sectie leest u hoe het configureren van een Azure Active Directory-service-principal te gebruiken voor het implementeren van uw toepassing in de actie na bouwen. U kunt service-principals toewijzen aan rollen in uw directory om te beperken van de machtigingen van de Jenkins-taak. 

Voor ontwikkel- en testomgevingen, kunt u Azure-referenties of het eindpunt voor Clusterbeheer om uw toepassing te implementeren. Zie voor meer informatie over het configureren van een eindpunt voor Clusterbeheer [configureren van de implementatie met behulp van de cluster-beheereindpunt](#configure-deployment-using-cluster-management-endpoint).   

1. Volg de stappen in een Azure Active Directory service-principal maken en toewijzen van machtigingen in uw Azure-abonnement, [gebruikt de portal voor het maken van een Azure Active Directory-toepassing en service-principal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal). Let op het volgende:

   * Tijdens de stappen in het onderwerp te volgen, moet kopiëren en opslaan van de volgende waarden: *Toepassings-ID*, *Toepassingssleutel*, *map-ID (Tenant-ID)*, en *abonnements-ID*. U moet de Azure-referenties in Jenkins worden geconfigureerd.
   * Als u geen de [vereiste machtigingen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) op uw directory gebruikt, moet u vraag een beheerder maken van de service-principal voor u of u de machtigingen te verlenen, of moet u het configureren van het eindpunt voor uw -cluster in de **acties na bouwen** voor uw Jenkins-taak.
   * In de [maken van een Azure Active Directory-toepassing](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal#create-an-azure-active-directory-application) sectie, kunt u een goed ingedeelde URL voor de **aanmeldings-URL**.
   * In de [toepassing toewijzen aan een rol](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal) sectie, kunt u uw toepassing de *lezer* -rol op de resourcegroep voor uw cluster.

1. Klik in de Jenkins-taak op de **acties na bouwen** tabblad.
1. Selecteer in de vervolgkeuzelijst **Post-Build Actions** de optie **Post-Build Actions**. 
1. Onder **configuratie van Service Fabric-Cluster**, selecteer de **selecteert u de Service Fabric-Cluster** keuzerondje. Klik op **toevoegen** naast **Azure Credentials**. Klik op **Jenkins** om de Jenkins-referenties-Provider te selecteren.
1. Selecteer in de Provider van de Jenkins-referenties, **Microsoft Azure Service-Principal** uit de **soort** vervolgkeuzelijst.
1. Gebruik de waarden die u hebt opgeslagen bij het instellen van uw service-principal in stap 1 om het instellen van de volgende velden:

   * **Client-ID**: *Toepassings-ID*
   * **Clientgeheim**: *Toepassingssleutel*
   * **Tenant-ID**: *Map-ID*
   * **Abonnements-ID**: *Subscription ID*
1. Voer een beschrijvende **ID** dat u gebruikt om te selecteren van de referentie in Jenkins en een korte **beschrijving**. Klik vervolgens op **Service-Principal verifiëren**. Als de verificatie slaagt, klikt u op **toevoegen**.

   ![Service Fabric Jenkins opgeven Azure-referenties](./media/service-fabric-cicd-your-linux-application-with-jenkins/enter-azure-credentials.png)
1. Onder **configuratie van Service Fabric-Cluster**, zorg ervoor dat uw nieuwe referentie is geselecteerd voor **Azure Credentials**. 
1. Uit de **resourcegroep** vervolgkeuzelijst, selecteer de resourcegroep van het cluster dat u wilt de toepassing te implementeren.
1. Uit de **Service Fabric** vervolgkeuzelijst, selecteert u het cluster dat u wilt de toepassing te implementeren.
1. Voor **Clientsleutel** en **clientcertificaat**, geef de locatie van het PEM-bestand in uw Jenkins-container. Bijvoorbeeld `/var/jenkins_home/clustercert.pem`. 
1. Onder **Toepassingsconfiguratie**, configureer de **toepassingsnaam**, **toepassingstype**, en de (relatief) **pad naar het Manifest van de toepassing** velden.
    ![Service Fabric Jenkins na Build-actie configureren Azure-referenties](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-credentials.png)
1. Klik op **configuratie controleren**. Klik op een geslaagde verificatie **opslaan**. De pijplijn van uw Jenkins-taak is nu volledig geconfigureerd. Gaat u naar [Vervolgstappen](#next-steps) om uw implementatie te testen.

## <a name="troubleshooting-the-jenkins-plugin"></a>Problemen met de Jenkins-invoegtoepassing oplossen

Als u problemen ondervindt met de Jenkins-invoegtoepassingen, kunt u in [Jenkins JIRA](https://issues.jenkins-ci.org/) een ticket openen voor het specifieke onderdeel.

## <a name="next-steps"></a>Volgende stappen
GitHub en Jenkins zijn nu geconfigureerd. U kunt een aantal voorbeelden wijzigen de `reliable-services-actor-sample/Actors/ActorCounter` project in de fork van de opslagplaats https://github.com/Azure-Samples/service-fabric-java-getting-started. Push uw wijzigingen naar de externe `master` vertakking (of een vertakking die u hebt geconfigureerd om te werken met). Op die manier wordt de geconfigureerde Jenkins-taak `MyJob` geactiveerd. Het haalt de wijzigingen vanuit GitHub, ze maakt en implementeert de toepassing op het cluster dat u hebt opgegeven in de acties na bouwen.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step.png
  [build-step-dotnet]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step-dotnet.png


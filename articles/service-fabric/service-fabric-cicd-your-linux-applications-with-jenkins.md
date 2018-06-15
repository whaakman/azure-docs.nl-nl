---
title: Continue build en de integratie voor uw Azure Service Fabric Linux-toepassingen met behulp van Jenkins | Microsoft Docs
description: Continue build en integratie voor uw Service Fabric Linux-toepassing met behulp van Jenkins
services: service-fabric
documentationcenter: java
author: sayantancs
manager: timlt
editor: ''
ms.assetid: 02b51f11-5d78-4c54-bb68-8e128677783e
ms.service: service-fabric
ms.devlang: java
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: saysa
ms.openlocfilehash: 047b3d00da4f192febeeab79c9c87b67a8a0489b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207958"
---
# <a name="use-jenkins-to-build-and-deploy-your-linux-applications"></a>Jenkins gebruiken om te bouwen en implementeren van uw Linux-toepassingen
Jenkins is een populair hulpprogramma voor doorlopende integratie en implementatie van uw apps. Hier leest u hoe u een Azure Service Fabric-toepassing maakt en implementeert met behulp van Jenkins.

## <a name="topic-overview"></a>Onderwerp-overzicht
In dit artikel bevat informatie over verschillende manieren van het instellen van uw omgeving Jenkins evenals andere manieren voor het implementeren van uw toepassing naar een Service Fabric-cluster nadat deze is opgebouwd. Volg deze algemene stappen voor het met succes Jenkins instellen, pull-wijzigingen vanuit GitHub, uw toepassing bouwen en deze implementeren voor uw cluster:

1. Zorg ervoor dat u installeert de [vereisten](#prerequisites).
2. Volg de stappen in een van deze secties Jenkins instellen:
   * [Jenkins instellen in een Service Fabric-cluster](#set-up-jenkins-inside-a-service-fabric-cluster), 
   * [Instellen van Jenkins buiten een Service Fabric-cluster](#set-up-jenkins-outside-a-service-fabric-cluster), of
   * [De Service Fabric-invoegtoepassing installeren in een omgeving met bestaande Jenkins](#install-service-fabric-plugin-in-an-existing-jenkins-environment).
3. Nadat u Jenkins hebt ingesteld, volg de stappen in [maken en configureren van een taak Jenkins](#create-and-configure-a-jenkins-job) voor het instellen van GitHub Jenkins activeren wanneer wijzigingen worden aangebracht in uw toepassing en het configureren van uw pijplijn Jenkins taak via de build stap voor het ophalen van de wijzigingen van GitHub en uw toepassing bouwen. 
4. Ten slotte de Jenkins taak na build stap voor het implementeren van uw toepassing met uw Service Fabric-cluster configureren. Er zijn twee manieren Jenkins voor het implementeren van uw toepassing naar een cluster te configureren:    
   * Gebruik voor ontwikkel- en testomgevingen [clustereindpunt management-implementatie configureren](#configure-deployment-using-cluster-management-endpoint). Dit is de eenvoudigste implementatiemethode om in te stellen.
   * Gebruik voor productieomgevingen [implementatie met behulp van Azure-referenties configureren](#configure-deployment-using-azure-credentials). Microsoft raadt aan om deze methode voor productieomgevingen omdat met de Azure-referenties kunt u de toegang van een taak Jenkins beperken voor uw Azure-resources. 

## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat Git lokaal is geïnstalleerd. U kunt installeren de juiste Git-versie van [de Git pagina met downloads](https://git-scm.com/downloads) op basis van uw besturingssysteem. Als u geen ervaring met Git, meer informatie over het uit de [Git documentatie](https://git-scm.com/docs).
- Dit artikel wordt de *Service Fabric ophalen gestart voorbeeld* op GitHub: [ https://github.com/Azure-Samples/service-fabric-java-getting-started ](https://github.com/Azure-Samples/service-fabric-java-getting-started) voor de toepassing te implementeren. U kunt deze repository te volgen vertakken of, met enkele wijzigingen aan de instructies, gebruik van uw eigen GitHub-project.


## <a name="install-service-fabric-plugin-in-an-existing-jenkins-environment"></a>Service Fabric-invoegtoepassing installeren in een bestaande Jenkins-omgeving
Als u de Service Fabric-invoegtoepassing aan een bestaande Jenkins-omgeving toevoegt, moet u het volgende:

- De [Service Fabric CLI](service-fabric-cli.md) (sfctl).

   > [!NOTE]
   > Zorg ervoor dat de CLI op systeemniveau installeren in plaats van op gebruikersniveau, dus Jenkins kunt CLI-opdrachten uitvoeren. 
   >

- Voor het implementeren van Java-toepassingen installeren [Gradle en Open JDK 8.0](service-fabric-get-started-linux.md#set-up-java-development). 
- Installeren .NET Core 2.0 om toepassingen te implementeren, de [SDK voor .NET Core 2.0](service-fabric-get-started-linux.md#set-up-net-core-20-development). 

Nadat u de vereisten die nodig zijn voor uw omgeving hebt geïnstalleerd, kunt u de Azure Service Fabric-invoegtoepassing in Jenkins marketplace zoeken en installeren.

Nadat u de invoegtoepassing hebt geïnstalleerd, gaat u verder met [maken en configureren van een taak Jenkins](#create-and-configure-a-jenkins-job).


## <a name="set-up-jenkins-inside-a-service-fabric-cluster"></a>Jenkins instellen in een Service Fabric-cluster

U kunt Jenkins instellen binnen of buiten een Service Fabric-cluster. De volgende secties ziet het instellen van binnen een cluster tijdens het gebruik van een Azure storage-account voor het opslaan van de status van het exemplaar van de container.

### <a name="prerequisites"></a>Vereisten
- Een Service Fabric Linux-cluster met Docker geïnstalleerd hebt. Service Fabric-clusters die al worden uitgevoerd in Azure hebben Docker geïnstalleerd. Als u lokaal het cluster (OneBox dev-omgeving) uitvoert, controleert u of Docker is geïnstalleerd op uw machine met de `docker info` opdracht. Als dit niet is geïnstalleerd, kunt u deze met behulp van de volgende opdrachten installeren:

   ```sh
   sudo apt-get install wget
   wget -qO- https://get.docker.io/ | sh
   ``` 

   > [!NOTE]
   > Zorg ervoor dat de poort 8081 is opgegeven als een aangepaste eindpunt op het cluster. Als u een lokaal cluster gebruikt, zorg ervoor dat poort 8081 geopend op de hostmachine is, en dat er een openbare IP-adres.
   >

### <a name="steps"></a>Stappen
1. Klonen van de toepassing met behulp van de volgende opdrachten:
   ```sh
   git clone https://github.com/suhuruli/jenkins-container-application.git
   cd jenkins-container-application
   ```

3. De status van de container Jenkins in een bestandsshare behouden:
   1. Maken van een Azure storage-account in de **dezelfde regio** als uw cluster met een naam zoals `sfjenkinsstorage1`.
   2. Maak een **bestandsshare** onder de storage-Account met een naam, zoals `sfjenkins`.
   3. Klik op **Connect** voor de bestandsshare en noteer de waarden wordt weergegeven onder **verbinding te maken van Linux**, de waarde moet er ongeveer als hieronder:

      ```sh
      sudo mount -t cifs //sfjenkinsstorage1.file.core.windows.net/sfjenkins [mount point] -o vers=3.0,username=sfjenkinsstorage1,password=<storage_key>,dir_mode=0777,file_mode=0777
      ```

   > [!NOTE]
   > Koppelpunt cifs shares moet u beschikken over de cifs-utils-pakket geïnstalleerd in de clusterknooppunten.      
   >

4. Werk de tijdelijke aanduiding voor waarden in de `setupentrypoint.sh` script met de azure-storage-gegevens uit stap 2.
   ```sh
   vi JenkinsSF/JenkinsOnSF/Code/setupentrypoint.sh
   ```
   * Vervang `[REMOTE_FILE_SHARE_LOCATION]` met de waarde `//sfjenkinsstorage1.file.core.windows.net/sfjenkins` uit de uitvoer van de verbinding te maken in stap 2 hierboven.
   * Vervang `[FILE_SHARE_CONNECT_OPTIONS_STRING]` met de waarde `vers=3.0,username=sfjenkinsstorage1,password=GB2NPUCQY9LDGeG9Bci5dJV91T6SrA7OxrYBUsFHyueR62viMrC6NIzyQLCKNz0o7pepGfGY+vTa9gxzEtfZHw==,dir_mode=0777,file_mode=0777` uit stap 2 hierboven.

5. **Alleen beveiligde Cluster:** 
   
   Voor het configureren van de implementatie van toepassingen op een beveiligde cluster van Jenkins, moet het certificaat van het cluster toegankelijk in de container Jenkins. In de *ApplicationManifest.xml* bestand, onder de **ContainerHostPolicies** tag deze certificaatverwijzing toevoegen en bijwerken van de vingerafdrukwaarde met die van het certificaat van het cluster.

   ```xml
   <CertificateRef Name="MyCert" X509FindValue="[Thumbprint]"/>
   ```

   Voeg bovendien de volgende regels onder de **ApplicationManifest** (hoofdlabel) in de *ApplicationManifest.xml* -bestand en de vingerafdrukwaarde bijwerken met die van het certificaat van het cluster.

   ```xml
   <Certificates>
     <SecretsCertificate X509FindType="FindByThumbprint" X509FindValue="[Thumbprint]" />
   </Certificates> 
   ```

6. Verbinding maken met het cluster en installeer de containertoepassing.

   **Beveiligde Cluster**
   ```sh
   sfctl cluster select --endpoint https://PublicIPorFQDN:19080  --pem [Pem] --no-verify # cluster connect command
   bash Scripts/install.sh
   ```
   De voorgaande opdracht wordt het certificaat in de PEM-indeling. Als uw certificaat in PFX-indeling, kunt u de volgende opdracht om deze te converteren. Als uw PFX-bestand niet is beveiligd met een wachtwoord, geeft u de **passin** parameter als `-passin pass:`.
   ```sh
   openssl pkcs12 -in cert.pfx -out cert.pem -nodes -passin pass:MyPassword1234!
   ``` 
   
   **Niet-beveiligde Cluster**
   ```sh
   sfctl cluster select --endpoint http://PublicIPorFQDN:19080 # cluster connect command
   bash Scripts/install.sh
   ```

   Hiermee wordt een Jenkins-container in het cluster geïnstalleerd. Dit kan worden bewaakt met de Service Fabric Explorer.

   > [!NOTE]
   > Het duurt een paar minuten voor de installatiekopie Jenkins worden gedownload op het cluster.
   >

7. Ga in uw browser naar `http://PublicIPorFQDN:8081`. Hier vindt u het pad naar het eerste beheerderswachtwoord dat nodig is om u aan te melden. 
2. Bekijk de Service Fabric Explorer om te bepalen welk knooppunt de container Jenkins wordt uitgevoerd. Secure Shell (SSH) aanmelden bij dit knooppunt.
   ```sh
   ssh user@PublicIPorFQDN -p [port]
   ``` 
3. Haal de exemplaar-id van de container op met `docker ps -a`.
4. Secure Shell (SSH) aanmelden bij de container en plak het pad dat u in de portal Jenkins zijn weergegeven. Bijvoorbeeld, als in de portal wordt het pad `PATH_TO_INITIAL_ADMIN_PASSWORD`, voer de volgende opdrachten:

   ```sh
   docker exec -t -i [first-four-digits-of-container-ID] /bin/bash   # This takes you inside Docker shell
   ```
   ```sh
   cat PATH_TO_INITIAL_ADMIN_PASSWORD # This displays the password value
   ```
5. Kies op de pagina Jenkins aan de slag de Selecteer invoegtoepassingen voor de installatie-optie, selecteert u de **geen** selectievakje in en klik op installeren.
6. Een gebruiker maken of selecteren om door te gaan als een beheerder.

Nadat u Jenkins hebt ingesteld, gaat u verder met [maken en configureren van een taak Jenkins](#create-and-configure-a-jenkins-job).  

## <a name="set-up-jenkins-outside-a-service-fabric-cluster"></a>Jenkins instellen buiten een Service Fabric-cluster

U kunt Jenkins instellen binnen of buiten een Service Fabric-cluster. In de volgende secties ziet u hoe u Jenkins instelt buiten een cluster.

### <a name="prerequisites"></a>Vereisten
- Zorg ervoor dat de Docker op uw computer is geïnstalleerd. De volgende opdrachten kunnen worden gebruikt om Docker vanuit de terminal te installeren:

  ```sh
  sudo apt-get install wget
  wget -qO- https://get.docker.io/ | sh
  ```

  Bij het uitvoeren van `docker info` in de terminal in de uitvoer moet blijken dat de Docker-service wordt uitgevoerd.

### <a name="steps"></a>Stappen
1. Haal de Service Fabric Jenkins container-installatiekopie op: `docker pull rapatchi/jenkins:latest`. Deze installatiekopie wordt geleverd met de Service Fabric Jenkins-invoegtoepassing die vooraf is geïnstalleerd.
2. Voer de installatiekopie van de container uit: `docker run -itd -p 8080:8080 rapatchi/jenkins:latest`
3. Haal de id op van het exemplaar van de installatiekopie van de container. U kunt een lijst van alle Docker-containers bekijken met de opdracht `docker ps –a`
4. Meld u aan bij de portal Jenkins met de volgende stappen:

   1. Aanmelden bij een shell Jenkins vanaf de host. Gebruik de eerste vier cijfers van de container-ID. Bijvoorbeeld, als de container-ID is `2d24a73b5964`, gebruik `2d24`.

      ```sh
      docker exec -it [first-four-digits-of-container-ID] /bin/bash
      ```
   2. Vanuit de shell Jenkins het beheerderswachtwoord voor uw exemplaar van de container niet verkrijgen:

      ```sh
      cat /var/jenkins_home/secrets/initialAdminPassword
      ```      
   3. Als u wilt aanmelden bij het dashboard Jenkins, opent u de volgende URL in een webbrowser: `http://<HOST-IP>:8080`. Het wachtwoord van de vorige stap voor het ontgrendelen van Jenkins gebruiken.
   4. (Optioneel.) Nadat u zich aanmeldt voor de eerste keer, u kunt uw eigen gebruikersaccount maken en gebruiken die voor de volgende stappen uit of u kunt blijven gebruiken van de administrator-account. Als u een gebruiker maken, moet u om door te gaan met die gebruiker.
5. GitHub instellen om te werken met Jenkins met behulp van de stappen in [genereren van een nieuwe SSH-sleutel en voegt deze toe aan de SSH-agent](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/).
   * Volg de instructies van GitHub om de SSH-sleutel te genereren en deze toe te voegen aan het GitHub-account waarop de opslagplaats wordt gehost.
   * Voer de opdrachten die in de vorige koppeling zijn genoemd, uit in de Jenkins Docker-shell (en niet op uw host).
   * Gebruik de volgende opdracht om u vanaf uw host aan te melden bij de Jenkins-shell:

      ```sh
      docker exec -t -i [first-four-digits-of-container-ID] /bin/bash
      ```

Zorg ervoor dat het cluster of de computer waarop de installatiekopie van de container Jenkins wordt gehost een openbare IP-adres heeft. Alleen zo kan het Jenkins-exemplaar meldingen ontvangen van GitHub.

Nadat u Jenkins hebt ingesteld, door te gaan naar de volgende sectie [maken en configureren van een taak Jenkins](#create-and-configure-a-jenkins-job).

## <a name="create-and-configure-a-jenkins-job"></a>Een Jenkins-taak maken en configureren

De stappen in deze sectie laten zien hoe een taak Jenkins om te reageren op wijzigingen in een GitHub-opslagplaats, ophalen van de wijzigingen en bouwen ze configureren. Aan het einde van deze sectie wordt u omgeleid naar de laatste stappen voor het configureren van de taak voor het implementeren van uw toepassing op basis van of u in een omgeving ontwikkeling en testen of voor een productieomgeving implementeert. 

1. Klik op het dashboard Jenkins **Nieuw Item**.
2. Voer een itemnaam in (bijvoorbeeld **MyJob**). Selecteer **free-style project** en klik op **OK**.
3. De configuratiepagina van de taak wordt geopend. (Als u de configuratie vanuit het dashboard Jenkins, klik op de taak en klik vervolgens op **configureren**).

4. Op de **algemene** tabblad, schakel het selectievakje voor **GitHub project**, en geef de URL van de GitHub-project. Dit is de URL waarop de Service Fabric Java-toepassing wordt gehost die u wilt integreren met de CI-/CD-stroom van Jenkins (Continue integratie, Continue implementatie), bijvoorbeeld `https://github.com/{your-github-account}/service-fabric-java-getting-started`.

5. Op de **Source Code Management** tabblad **Git**. Geef de opslagplaats-URL op waarop de Service Fabric Java-toepassing wordt gehost die u wilt integreren met de CI-/CD-stroom van Jenkins, bijvoorbeeld `https://github.com/{your-github-account}/service-fabric-java-getting-started`. U kunt ook opgeven welke vertakking om samen te stellen (bijvoorbeeld `/master`).
6. Configureer uw *GitHub* te communiceren met Jenkins opslagplaats:

   a. Ga op uw GitHub-opslagplaats-pagina naar **instellingen** > **integraties en Services**.

   b. Selecteer **Add Service**, typ **Jenkins** en selecteer de **Jenkins-GitHub-invoegtoepassing**.

   c. Geef de Jenkins-webhook-URL op (standaard is dit `http://<PublicIPorFQDN>:8081/github-webhook/`). Klik op **add/update service**.

   d. Er wordt een testgebeurtenis verzonden naar uw Jenkins-exemplaar. Er komt een groen vinkje bij de webhook te staan in GitHub en uw project wordt gemaakt.

7. Op de **bouwen Triggers** tabblad Jenkins, selecteert u die optie die u wilt maken. Bijvoorbeeld, die u wilt een build activeren telkens wanneer een push naar de opslagplaats gebeurt, dus selecteer **GitHub haakje trigger voor GITScm polling**. (Voorheen heette deze optie **Build when a change is pushed to GitHub**.)
8. Op de **bouwen** tabblad en voer een van de volgende, afhankelijk van of u een Java-toepassing of een toepassing .NET Core maakt:

   * **Voor Java-toepassingen:** van de **toevoegen build stap** Vervolgkeuzelijst, selecteer **aanroepen Gradle Script**. Klik op **geavanceerde**. Geef het pad naar in het menu Geavanceerd **hoofdmap build script** voor uw toepassing. Het haalt build.gradle op uit het opgegeven pad en werkt dienovereenkomstig. Voor de [ActorCounter toepassing](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/reliable-services-actor-sample/Actors/ActorCounter), dit is: `${WORKSPACE}/reliable-services-actor-sample/Actors/ActorCounter`.

     ![Service Fabric-bouwactie voor Jenkins][build-step]

   * **Voor .NET Core-toepassingen:** van de **toevoegen build stap** Vervolgkeuzelijst, selecteer **Shell uitvoeren**. In het vak opdracht moet de map eerst moet worden gewijzigd in het pad naar het bestand build.sh. Zodra de map is gewijzigd, wordt het script build.sh kan worden uitgevoerd en wordt de toepassing wordt opgebouwd.

      ```sh
      cd /var/jenkins_home/workspace/[Job Name]/[Path to build.sh]  # change directory to location of build.sh file
      ./build.sh
      ```

     De volgende Schermafbeelding toont een voorbeeld van de opdrachten die worden gebruikt om samen te stellen de [teller Service](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started/tree/master/Services/CounterService) voorbeeld met een naam van de taak Jenkins van CounterServiceApplication.

      ![Service Fabric-bouwactie voor Jenkins][build-step-dotnet]

9. Als u wilt configureren Jenkins uw app implementeren op een Service Fabric-cluster in de acties na het samenstellen, moet u de locatie van certificaat van het cluster in uw Jenkins-container. Kies een van de volgende, afhankelijk van of uw Jenkins-container binnen of buiten uw cluster wordt uitgevoerd en noteer de locatie van het certificaat van het cluster:

   * **Voor Jenkins uitgevoerd binnen het cluster:** het pad naar het certificaat kunt u vinden door de waarde van echo de *Certificates_JenkinsOnSF_Code_MyCert_PEM* omgevingsvariabele uit in de container.

      ```sh
      echo $Certificates_JenkinsOnSF_Code_MyCert_PEM
      ```
   
   * **Voor Jenkins uitgevoerd buiten het cluster:** Volg deze stappen voor het certificaat cluster kopiëren naar de container:
      1. Het certificaat moet zich in PEM-indeling. Als u een PEM-bestand niet hebt, kunt u een van de PFX-certificaatbestand. Als uw PFX-bestand niet beveiligd met een wachtwoord is, voert u de volgende opdracht vanaf de host:

         ```sh
         openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:
         ``` 

      Als het PFX-bestand beveiligd met een wachtwoord is, voegt u het wachtwoord in de `-passin` parameter. Bijvoorbeeld:

         ```sh
         openssl pkcs12 -in clustercert.pfx -out clustercert.pem -nodes -passin pass:MyPassword1234!
         ``` 

      2. Uitvoeren als u de container-ID voor de container Jenkins, `docker ps` van de host.
      3. Kopieer het PEM-bestand naar de container met de volgende Docker-opdracht:
    
         ```sh
         docker cp clustercert.pem [first-four-digits-of-container-ID]:/var/jenkins_home
         ``` 

U bent bijna klaar! Houd de taak Jenkins open. De enige resterende taak is voor het configureren van de stappen na het samenstellen voor het implementeren van uw toepassing met uw Service Fabric-cluster:

* Als u wilt implementeren op een ontwikkel- of -omgeving, volg de stappen in [clustereindpunt management-implementatie configureren](#configure-deployment-using-cluster-management-endpoint).
* Als u wilt implementeren in een productieomgeving, volg de stappen in [implementatie met behulp van Azure-referenties configureren](#configure-deployment-using-azure-credentials).

## <a name="configure-deployment-using-cluster-management-endpoint"></a>Implementatie met behulp van clustereindpunt management configureren
Voor ontwikkel- en testomgevingen, kunt u het cluster management-eindpunt om uw toepassing te implementeren. De actie na build configureren met het eindpunt van de cluster management op het implementeren van uw toepassing vereist zo min mogelijk configuratie. Als u in een productieomgeving implementeert, gaat u verder met [implementatie met behulp van Azure-referenties configureren](#configure-deployment-using-azure-credentials) voor het configureren van een Azure Active Directory-service-principal moet worden gebruikt tijdens de implementatie.    

1. Klik in de taak Jenkins op de **na build acties** tabblad. 
2. Selecteer in de vervolgkeuzelijst **Post-Build Actions** de optie **Post-Build Actions**. 
3. Onder **Service Fabric-clusterconfiguratie**, selecteer de **vult u het Service-eindpunt voor het beheer van Fabric** keuzerondje.
4. Voor **Management Host**, voer het verbindingseindpunt voor uw cluster; bijvoorbeeld `{your-cluster}.eastus.cloudapp.azure.com`.
5. Voor **Clientsleutel** en **Client Cert**, geef de locatie van het PEM-bestand in de container Jenkins; bijvoorbeeld `/var/jenkins_home/clustercert.pem`. (U hebt de laatste stap van de locatie van het certificaat gekopieerd [maken en configureren van een taak Jenkins](#create-and-configure-a-jenkins-job).)
6. Onder **Toepassingsconfiguratie**, configureren de **toepassingsnaam**, **toepassingstype**, en de (relatief) **pad naar Application Manifest** velden.

   ![Service Fabric Jenkins na Build actie configureren management-eindpunt](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-endpoint.png)

7. Klik op **configuratie controleren**. Klik op een geslaagde verificatie **opslaan**. Uw taak Jenkins pijplijn is nu volledig geconfigureerd. Gaat u verder met [Vervolgstappen](#next-steps) om uw implementatie te testen.

## <a name="configure-deployment-using-azure-credentials"></a>Implementatie met behulp van Azure-referenties configureren
Voor productieomgevingen, wordt configureren van een Azure-referentie voor het implementeren van uw toepassing sterk aanbevolen. Deze sectie leest u over het configureren van een Azure Active Directory-service-principal te gebruiken om uw toepassing in de actie na build te implementeren. U kunt service-principals toewijzen aan rollen in uw directory de machtigingen van de taak Jenkins wilt beperken. 

U kunt Azure-referenties of het cluster management eindpunt voor het implementeren van uw toepassing configureren voor ontwikkel- en testomgevingen. Zie voor meer informatie over het configureren van een cluster management eindpunt [clustereindpunt management-implementatie configureren](#configure-deployment-using-cluster-management-endpoint).   

1. Als u een Azure Active Directory-service-principal maken en deze machtigingen in uw Azure-abonnement toewijzen, wilt u de stappen in [de portal gebruiken om een Azure Active Directory-toepassing en service-principal maken](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal). Let op het volgende:

   * Tijdens de stappen in het onderwerp te volgen, moet u kopiëren en opslaan van de volgende waarden: *toepassings-ID*, *Toepassingssleutel*, *map-ID (Tenant-ID)*, en *Abonnements-ID*. U moet ze configureren van de Azure-referenties in Jenkins.
   * Als u hebt de [vereist machtigingen](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#required-permissions) op uw directory, moet u vraag een beheerder maken van de service-principal voor u of u de machtigingen verlenen of moet u de management eindpunt configureert voor uw -cluster in de **na Build acties** voor de taak in Jenkins.
   * In de [maken van een Azure Active Directory-toepassing](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#create-an-azure-active-directory-application) sectie, kunt u elke goed ingedeelde URL voor de **aanmeldings-URL**.
   * In de [toepassing toewijzen aan een rol](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-create-service-principal-portal#assign-application-to-role) sectie, kunt u uw toepassing de *lezer* rol in de resourcegroep voor uw cluster.

2. Klik in de taak Jenkins op de **na build acties** tabblad.
3. Selecteer in de vervolgkeuzelijst **Post-Build Actions** de optie **Post-Build Actions**. 
4. Onder **Service Fabric-clusterconfiguratie**, selecteer de **selecteert u het Service Fabric-Cluster** keuzerondje. Klik op **toevoegen** naast **Azure-referenties**. Klik op **Jenkins** om de referenties Jenkins Provider te selecteren.
5. Selecteer in de Provider van de referenties Jenkins **Microsoft Azure Service-Principal** van de **soort** vervolgkeuzelijst.
6. Gebruik de waarden die u hebt opgeslagen bij het instellen van uw service-principal in stap 1 om het instellen van de volgende velden:

   * **Client-ID**: *toepassings-ID*
   * **Clientgeheim**: *Toepassingssleutel*
   * **Tenant-ID**: *map-ID*
   * **Abonnements-ID**: *abonnements-ID*
6. Voer een beschrijvende **ID** waarmee u kunt selecteren van de referentie in Jenkins en een korte **beschrijving**. Klik vervolgens op **Controleer of Service-Principal**. Als de verificatie slaagt, klikt u op **toevoegen**.

   ![Service Fabric Jenkins invoeren Azure-referenties](./media/service-fabric-cicd-your-linux-application-with-jenkins/enter-azure-credentials.png)
7. Back-onder **Service Fabric-clusterconfiguratie**, zorg ervoor dat uw nieuwe referenties is geselecteerd voor **Azure-referenties**. 
8. Van de **resourcegroep** vervolgkeuzelijst, selecteert u de resourcegroep van het cluster dat u de toepassing te implementeren.
9. Van de **Service Fabric** vervolgkeuzelijst, selecteer het cluster dat u wilt de toepassing te implementeren.
10. Voor **Clientsleutel** en **Client Cert**, geef de locatie van het PEM-bestand in uw Jenkins-container. Bijvoorbeeld `/var/jenkins_home/clustercert.pem`. 
11. Onder **Toepassingsconfiguratie**, configureren de **toepassingsnaam**, **toepassingstype**, en de (relatief) **pad naar Application Manifest** velden.
    ![Service Fabric Jenkins na Build actie configureren Azure-referenties](./media/service-fabric-cicd-your-linux-application-with-jenkins/post-build-credentials.png)
12. Klik op **configuratie controleren**. Klik op een geslaagde verificatie **opslaan**. Uw taak Jenkins pijplijn is nu volledig geconfigureerd. Gaan naar [Vervolgstappen](#next-steps) om uw implementatie te testen.

## <a name="next-steps"></a>Volgende stappen
GitHub en Jenkins zijn nu geconfigureerd. Overweeg aantal voorbeelden wijzigen de `reliable-services-actor-sample/Actors/ActorCounter` -project in uw fork van de opslagplaats https://github.com/Azure-Samples/service-fabric-java-getting-started. Uw wijzigingen naar de externe push `master` vertakking (of een tak die u hebt geconfigureerd om te werken met). Op die manier wordt de geconfigureerde Jenkins-taak `MyJob` geactiveerd. Deze haalt de wijzigingen van GitHub, ze maakt en implementeert de toepassing op het cluster dat u hebt opgegeven in de acties na het samenstellen.  

  <!-- Images -->
  [build-step]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step.png
  [build-step-dotnet]: ./media/service-fabric-cicd-your-linux-application-with-jenkins/build-step-dotnet.png


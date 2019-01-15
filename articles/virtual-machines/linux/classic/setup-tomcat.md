---
title: Instellen van Apache Tomcat op een Linux-machine | Microsoft Docs
description: Leer hoe u Apache Tomcat7 instellen met behulp van Azure Virtual Machines waarop Linux wordt uitgevoerd.
services: virtual-machines-linux
documentationcenter: ''
author: NingKuang
manager: jeconnoc
editor: ''
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
ms.openlocfilehash: 5a5d052052be447ea2ccbd9231d3b03d38c7615c
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266940"
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a>Tomcat7 instellen op een Linux-machine met Azure
Apache Tomcat (of gewoon gebruik van Tomcat, ook voorheen Jakarta Tomcat) is een open-source-webserver en servletcontainer die zijn ontwikkeld door de Apache Software Foundation (AVP). Tomcat implementeert de Java Servlet- en JavaServer Pages (JSP) specificaties van Sun Microsystems. Tomcat biedt een zuivere Java HTTP web server-omgeving waarin u Java-code uit te voeren. In de eenvoudigste configuratie, Tomcat uitgevoerd in een bepaald besturingssysteem-proces. Dit proces wordt uitgevoerd een Java virtual machine (JVM). Elke HTTP-aanvraag vanuit een browser Tomcat wordt verwerkt als een afzonderlijke thread in de Tomcat-proces.  

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Azure Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over het gebruik van het klassieke implementatiemodel. We raden aan dat de meeste nieuwe implementaties het Resource Manager-model gebruiken. Als u een Resource Manager-sjabloon wilt implementeren van een Ubuntu-VM met Open JDK en Tomcat, Zie [in dit artikel](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

In dit artikel hebt u Tomcat7 installeren op een Linux-installatiekopie en implementeren in Azure.  

U leert:  

* Het maken van een virtuele machine in Azure.
* Klik hier voor meer informatie over het voorbereiden van de virtuele machine voor Tomcat7.
* Het Tomcat7 installeren.

Ervan wordt uitgegaan dat u al een Azure-abonnement hebt.  Als u niet het geval is, u kunt zich registreren voor een gratis proefversie op [de Azure-website](https://azure.microsoft.com/). Als u een MSDN-abonnement hebt, raadpleegt u [speciale prijzen voor Microsoft Azure: MSDN, MPN en BizSpark-voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Zie voor meer informatie over Azure, [wat is Azure?](https://azure.microsoft.com/overview/what-is-azure/).

In dit artikel wordt ervan uitgegaan dat u een werkende basiskennis hebben van Tomcat- en Linux.  

## <a name="phase-1-create-an-image"></a>Fase 1: Een installatiekopie maken
In deze fase maakt u een virtuele machine met behulp van een installatiekopie van Linux in Azure.  

### <a name="step-1-generate-an-ssh-authentication-key"></a>Stap 1: Een SSH-sleutel voor verificatie
SSH is een belangrijk hulpprogramma voor systeembeheerders. Configureren van de toegangsbeveiliging op basis van een wachtwoord human bepaald wordt echter niet aanbevolen. Kwaadwillende gebruikers kunnen in uw systeem op basis van een gebruikersnaam en een zwak wachtwoord opsplitsen.

Het goede nieuws is dat er een manier te laten RAS openen en u geen zorgen over wachtwoorden. Deze methode bestaat uit de verificatie met asymmetrische cryptografie. De persoonlijke sleutel van de gebruiker is het account waarmee de verificatie wordt verleend. U kunt zelfs van het gebruikersaccount niet toestaat wachtwoordverificatie vergrendelen.

Een ander voordeel van deze methode is dat u niet verschillende wachtwoorden hoeft te melden bij verschillende servers. U kunt verifiëren met behulp van de persoonlijke persoonlijke sleutel op alle servers, waarmee wordt voorkomen dat u met verschillende wachtwoorden onthouden.



Volg deze stappen voor het genereren van de SSH-sleutel voor verificatie.

1. Download en installeer PuTTYgen uit de volgende locatie: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Puttygen.exe worden uitgevoerd.
3. Klik op **genereren** om de sleutels te genereren. In het proces kunt u aanvraaggrootte verhogen door de muis over de leeg gebied in het venster verplaatsen.  
   ![PuTTY-Sleutelgenerator schermafdruk van de knop genereren nieuwe sleutel][1]
4. Nadat het proces genereren ziet Puttygen.exe u de nieuwe openbare sleutel.  
   ![PuTTY-Sleutelgenerator schermafdruk van de nieuwe openbare sleutel en het opslaan persoonlijke sleutel knop][2]
5. Selecteer en kopieer de openbare sleutel en opslaan in een bestand met de naam publicKey.pem. Klik niet op **openbare sleutel opslaan**, omdat de bestandsindeling opgeslagen van de openbare sleutel verschilt van de openbare sleutel die we willen.
6. Klik op **persoonlijke sleutel opslaan**, en sla deze in een bestand met de naam privateKey.ppk.

### <a name="step-2-create-the-image-in-the-azure-portal"></a>Stap 2: Maken van de installatiekopie in Azure portal
1. In de [portal](https://portal.azure.com/), klikt u op **een resource maken** in de taakbalk om een installatiekopie te maken. Kies vervolgens de Linux-installatiekopie die is gebaseerd op uw behoeften. Het volgende voorbeeld wordt de installatiekopie van het Ubuntu 14.04.
![Schermafbeelding van de portal die de nieuwe knop wordt weergegeven][3]

2. Voor **hostnaam**, geef de naam op voor de URL die u en de Internet-clients gebruiken voor toegang tot deze virtuele machine. Het laatste deel van de DNS-naam, bijvoorbeeld tomcatdemo definiëren. Azure wordt de URL wordt gegenereerd als tomcatdemo.cloudapp.net.  

3. Voor **SSH verificatiesleutel**, Kopieer de sleutelwaarde uit het bestand publicKey.pem, waarin de openbare sleutel die is gegenereerd door PuTTYgen.  
![SSH verificatiesleutel vak in de portal][4]

4. Configureer andere instellingen naar behoefte, en klik vervolgens op **maken**.  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Fase 2: Uw virtuele machine voorbereiden voor Tomcat7
In deze fase, wordt u een eindpunt configureert voor Tomcat-verkeer en maak verbinding met uw nieuwe virtuele machine.

### <a name="step-1-open-the-http-port-to-allow-web-access"></a>Stap 1: Open de HTTP-poort voor webtoegang
Eindpunten in Azure bestaan uit een TCP of UDP-protocol, samen met een openbare en particuliere poort. De particuliere poort is de poort die de service op de virtuele machine luistert. De openbare poort is de poort die de Azure-cloudservice naar extern voor binnenkomend verkeer op basis van het Internet luistert.  

TCP-poort 8080 is het standaardpoortnummer die gebruikmaakt van Tomcat om te luisteren. Als deze poort wordt geopend met een Azure-eindpunt, u en andere internetclients hebben toegang tot Tomcat-pagina's.  

1. Klik in de portal op **Bladeren** > **virtuele machines**, en klik vervolgens op de virtuele machine die u hebt gemaakt.  
   ![Schermafbeelding van de directory van de virtuele machines][5]
2. Als u wilt een eindpunt toevoegen aan uw virtuele machine, klikt u op de **eindpunten** vak.
   ![Schermafbeelding van het vak eindpunten][6]
3. Klik op **Add**.  

   1. Voer een naam in voor het eindpunt in voor het eindpunt, **eindpunt**, en voer vervolgens 80 in **openbare poort**.  

      Als u dit op 80 instellen, moet u niet het poortnummer dat is opgenomen in de URL die wordt gebruikt voor toegang tot Tomcat. Bijvoorbeeld http://tomcatdemo.cloudapp.net.    

      Als u dit op een andere waarde, zoals 81 instellen, moet u het poortnummer dat aan de URL voor toegang tot Tomcat toevoegen. Bijvoorbeeld http://tomcatdemo.cloudapp.net:81/.
   2. Voer 8080 in **particuliere poort**. Standaard luistert Tomcat op TCP-poort 8080. Als u de standaardwaarde gewijzigd van Tomcat-poort luistert, moet u bijwerken **particuliere poort** moet gelijk zijn aan de Tomcat poort voor luisteren.  
      ![Schermopname van gebruikersinterface waarin de opdracht, openbare poort en particuliere poort toevoegen][7]
4. Klik op **OK** het eindpunt toevoegen aan uw virtuele machine.

### <a name="step-2-connect-to-the-image-you-created"></a>Stap 2: Verbinding maken met de installatiekopie die u hebt gemaakt
U kunt een SSH-hulpprogramma om verbinding met uw virtuele machine te kiezen. In dit voorbeeld gebruiken we PuTTY.  

1. De DNS-naam van uw virtuele machine ophalen uit de portal.
    1. Klik op **Bladeren** > **virtuele machines**.
    2. Selecteer de naam van uw virtuele machine en klik vervolgens op **eigenschappen**.
    3. In de **eigenschappen** tegel, kijkt u in de **domeinnaam** vak om op te halen van de DNS-naam.  

2. Ophalen van het poortnummer voor SSH-verbindingen van de **SSH** vak.  
![Schermafbeelding van het poortnummer voor SSH-verbinding][8]

3. Download [PuTTY](http://www.putty.org/).  

4. Nadat u hebt gedownload, klikt u op het uitvoerbare bestand Putty.exe. In de PuTTY-configuratie, de basisopties configureren met de naam van de host en het poortnummer dat is verkregen van de eigenschappen van uw virtuele machine.   
![Schermafbeelding van de PuTTY-configuratie hosten opties voor naam en het poortnummer][9]

5. Klik in het linkerdeelvenster op **verbinding** > **SSH** > **Auth**, en klik vervolgens op **Bladeren** om op te geven de locatie van het bestand privateKey.ppk. Het bestand privateKey.ppk bevat de persoonlijke sleutel die wordt gegenereerd door PuTTYgen eerder in de ' fase 1: Een installatiekopie maken' sectie van dit artikel.  
![Schermafbeelding van de mapstructuur van de verbinding en knop Bladeren][10]

6. Klik op **Openen**. U kunt door een berichtvenster worden gewaarschuwd. Als u hebt geconfigureerd de DNS-naam en het poortnummer juist, klikt u op **Ja**.
![Schermafbeelding van de melding][11]

7. U wordt gevraagd uw gebruikersnaam invoeren.  
![Schermopname die laat waar zien u de gebruikersnaam invoeren][12]

8. Voer de gebruikersnaam die u gebruikt voor het maken van de virtuele machine in de ' fase 1: Een installatiekopie maken' sectie eerder in dit artikel. U ziet er ongeveer als volgt uit:  
![Schermafbeelding van de verificatie-bevestiging][13]

## <a name="phase-3-install-software"></a>Fase 3: Software installeren
In deze fase installeert u de Java runtime environment Tomcat7 en andere onderdelen Tomcat7.  

### <a name="java-runtime-environment"></a>Java runtime-omgeving
Tomcat is geschreven in Java. Zie [Azure ondersteund JDK](https://aka.ms/azure-jdks) voor meer informatie over het ophalen van volledig Java runtimes ondersteunde. U kunt ook brengt uw eigen, maar de rest van dit artikel gebruikt de versies van de Azure wordt ondersteund.


#### <a name="install-azure-supported-jdk"></a>Azure ondersteunde JDK installeren

Ga als volgt de `apt-get` installatie-instructies die worden beschreven op de [Azul Zulu Enterprise voor Azure](https://www.azul.com/downloads/azure-only/zulu/#apt-repo) website.

#### <a name="confirm-that-java-installation-is-successful"></a>Bevestig dat Java-installatie voltooid is
U kunt een opdracht als volgt gebruiken om te controleren of de Java-runtime-omgeving juist is geïnstalleerd:  
    Java-versie  

U ziet een bericht als volgt uit: ![Bericht OpenJDK installatie is voltooid][14]


### <a name="install-tomcat7"></a>Tomcat7 installeren
Gebruik de volgende opdracht uit om te Tomcat7 installeren.  

    sudo apt-get install tomcat7  

Als u niet Tomcat7 gebruikt, gebruikt u de juiste variatie van deze opdracht.  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a>Bevestig dat Tomcat7 installatie geslaagd is
Als u wilt controleren als Tomcat7 is geïnstalleerd, bladert u naar de DNS-naam van de Tomcat-server. In dit artikel wordt de voorbeeld-URL is http://tomcatexample.cloudapp.net/. Als u een bericht als het volgende ziet, Tomcat7 juist is geïnstalleerd.
![Bericht Tomcat7 installatie is voltooid][16]

### <a name="install-other-tomcat7-components"></a>Andere onderdelen Tomcat7 installeren
Er zijn andere optionele Tomcat-onderdelen die u kunt installeren.  

Gebruik de **sudo apt-cache, search tomcat7** opdracht om alle beschikbare componenten te zien. Gebruik de volgende opdrachten om enkele nuttige onderdelen te installeren.  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools to create user instances  

## <a name="phase-4-configure-tomcat7"></a>Fase 4: Tomcat7 configureren
In deze fase, kunt u Tomcat beheren.

### <a name="start-and-stop-tomcat7"></a>Starten en stoppen Tomcat7
De Tomcat7-server wordt automatisch gestart wanneer u deze installeert. U kunt deze ook starten met de volgende opdracht:   

    sudo /etc/init.d/tomcat7 start

Tomcat7 stoppen:

    sudo /etc/init.d/tomcat7 stop

De status van Tomcat7 weergeven:

    sudo /etc/init.d/tomcat7 status

Tomcat services opnieuw starten: 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a>Tomcat7 beheer
U kunt het configuratiebestand voor gebruikers van Tomcat voor het instellen van de referenties van beheerder bewerken. Gebruik de volgende opdracht:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Hier volgt een voorbeeld:  
![Schermafbeelding van de uitvoer van de sudo vi-opdracht][17]  

> [!NOTE]
> Maak een sterk wachtwoord voor de gebruikersnaam van de beheerder.  

Nadat u hebt dit bestand bewerkt, moet u Tomcat7 services met de volgende opdracht uit om ervoor te zorgen dat de wijzigingen van kracht opnieuw opstarten:  

    sudo /etc/init.d/tomcat7 restart  

Open uw browser en voer **http://<your tomcat server DNS name>/manager/html** als de URL. Voor het voorbeeld in dit artikel wordt de URL is http://tomcatexample.cloudapp.net/manager/html.  

Nadat u verbinding hebt moet u iets zien die vergelijkbaar is met het volgende:  
![Schermafbeelding van de Tomcat Web Application Manager][18]

## <a name="common-issues"></a>Algemene problemen
### <a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Geen toegang tot de virtuele machine met Tomcat en Moodle vanaf Internet
#### <a name="symptom"></a>Symptoom  
  Tomcat wordt uitgevoerd, maar kunt u de standaardpagina van Tomcat met uw browser niet zien.
#### <a name="possible-root-cause"></a>Mogelijke hoofdoorzaken   

  * De Tomcat listen-poort is niet gelijk zijn aan de particuliere poort van het eindpunt van de virtuele machine voor Tomcat-verkeer.  

     Controleer uw openbare poort en particuliere poort eindpuntinstellingen en zorg ervoor dat de particuliere poort is dat hetzelfde als de Tomcat poort voor luisteren. Zie "fase 1: Een installatiekopie maken' sectie van dit artikel voor meer informatie over het configureren van eindpunten voor uw virtuele machine.  

     Open om te bepalen de listen-poort van Tomcat, /etc/httpd/conf/httpd.conf (Red Hat-release), of /etc/tomcat7/server.xml (Debian-versie). Standaard is de Tomcat listen-poort 8080. Hier volgt een voorbeeld:  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     Als u een virtuele machine, zoals Debian of Ubuntu en u wijzigen van de standaard poort wilt van Tomcat luisteren (bijvoorbeeld 8081) gebruikt, moet u ook de poort voor het besturingssysteem te openen. Eerst opent u het profiel:  

        sudo vi /etc/default/tomcat7  

     Vervolgens Verwijder opmerkingen bij de laatste regel en wijzig 'Nee' op 'Ja'.  

        AUTHBIND=yes
  2. De firewall heeft het luisteren naar poort van Tomcat uitgeschakeld.

     U ziet alleen de standaard Tomcat pagina van de lokale host. Het probleem wordt waarschijnlijk dat de poort die wordt geluisterd op Tomcat, wordt geblokkeerd door de firewall. U kunt het hulpprogramma w3m gebruiken om te bladeren op de webpagina. De volgende opdrachten w3m installeren en blader naar de pagina van de standaard Tomcat:  


        sudo yum install w3m w3m-img


        w3m http://localhost:8080  
#### <a name="solution"></a>Oplossing

  * Als de Tomcat luisteren poort is niet gelijk zijn aan de particuliere poort van het eindpunt voor het verkeer naar de virtuele machine, moet u de particuliere poort om dat hetzelfde als de Tomcat poort voor luisteren.   
  2. Als het probleem wordt veroorzaakt door de firewall/iptables, moet u de volgende regels toevoegen aan /etc/sysconfig/iptables. De tweede regel is alleen nodig voor https-verkeer:  

      -A -p tcp -m tcp--port 80 -j accepteren invoer

      -A -p tcp -m tcp--Port 443 -j accepteren invoer  

     > [!IMPORTANT]
     > Zorg ervoor dat de vorige regels worden geplaatst boven de regels die wereldwijd van toegang, zoals het volgende beperken zou: - een invoer -j afwijzen--afwijzen-met een icmp-host-verboden



Als u wilt de iptables laden, moet u de volgende opdracht uitvoeren:

    service iptables restart

Dit is getest op CentOS 6.3.

### <a name="permission-denied-when-you-upload-project-files-to-varlibtomcat7webapps"></a>Machtiging is geweigerd wanneer u bestanden naar /var/lib/tomcat7/webapps uploadt /
#### <a name="symptom"></a>Symptoom
  Wanneer u een SFTP-client (zoals FileZilla) verbinding maken met uw virtuele machine en navigeert u naar /var/lib/tomcat7/webapps/naar de site publiceert, krijgt u een foutmelding die vergelijkbaar is met het volgende:  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a>Mogelijke hoofdoorzaken
  U hebt geen toestemming voor toegang tot de map /var/lib/tomcat7/webapps.  
#### <a name="solution"></a>Oplossing  
  U moet machtiging ophalen van het root-account. U kunt het eigendom van die map wijzigen van de hoofdmap voor de gebruikersnaam die u hebt gebruikt toen u de machine ingericht. Hier volgt een voorbeeld met de naam van het azureuser:  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  De optie -R om toe te passen de machtigingen voor alle bestanden in een map te gebruiken.  

  Met deze opdracht werkt ook voor mappen. De optie -R wordt de machtigingen voor alle bestanden en mappen in de map. Hier volgt een voorbeeld:  

     sudo chown -R username:group directory  

  Deze opdracht eigendom (gebruikers en groepen) voor alle bestanden en mappen die zich in de map wordt gewijzigd.  

  De volgende opdracht worden alleen de machtiging van de mapdirectory wijzigt. De bestanden en mappen in de map worden niet gewijzigd.  

     sudo chown username:group directory

[1]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]:media/setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png

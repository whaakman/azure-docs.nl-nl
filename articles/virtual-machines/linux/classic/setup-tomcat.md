---
title: Apache Tomcat instellen op een virtuele Linux-machine | Microsoft Docs
description: Informatie over het instellen van Apache Tomcat7 met behulp van Azure Virtual Machines waarop Linux wordt uitgevoerd.
services: virtual-machines-linux
documentationcenter: 
author: NingKuang
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 45ecc89c-1cb0-4e80-8944-bd0d0bbedfdc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 12/15/2015
ms.author: ningk
ms.openlocfilehash: b76f6f7f53bc649fbc740a79e182f81f3fd983c7
ms.sourcegitcommit: ce934aca02072bdd2ec8d01dcbdca39134436359
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2017
---
# <a name="set-up-tomcat7-on-a-linux-virtual-machine-with-azure"></a>Tomcat7 instellen op een virtuele Linux-machine met Azure
Apache Tomcat (of gewoon Tomcat, ook voorheen ondersteuning Tomcat) is een open-source-webserver en de servlet-container die is ontwikkeld door de Apache Software Foundation (AVP). Tomcat implementeert de Servlet Java en de specificaties Java Server Pages (JSP) van Sun Microsystems. Tomcat biedt een pure Java HTTP web server-omgeving waarin u Java-code uit te voeren. In de eenvoudigste configuratie, Tomcat uitgevoerd in een proces één besturingssysteem. Dit proces wordt uitgevoerd voor een virtuele Java-machine (JVM). Elke HTTP-aanvraag via een browser Tomcat wordt als een afzonderlijke thread in het proces Tomcat verwerkt.  

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Azure Resource Manager en classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over het gebruik van het klassieke implementatiemodel. U wordt aangeraden de meeste nieuwe implementaties het Resource Manager-model gebruiken. Als u een Resource Manager-sjabloon voor het implementeren van een VM Ubuntu met Open JDK en Tomcat, Zie [in dit artikel](https://azure.microsoft.com/documentation/templates/openjdk-tomcat-ubuntu-vm/).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

In dit artikel leert u Tomcat7 installeren op een Linux-installatiekopie en deze implementeren in Azure.  

U leert het volgende:  

* Het maken van een virtuele machine in Azure.
* Klik hier voor meer informatie over het voorbereiden van de virtuele machine voor Tomcat7.
* Het Tomcat7 installeren.

Ervan wordt uitgegaan dat u al een Azure-abonnement hebt.  Als u niet, u kunt zich aanmelden voor een gratis proefversie op [de Azure-website](https://azure.microsoft.com/). Als u een MSDN-abonnement hebt, raadpleegt u [speciale prijzen voor Microsoft Azure: MSDN, MPN en BizSpark voordelen](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Zie voor meer informatie over Azure, [wat is Azure?](https://azure.microsoft.com/overview/what-is-azure/).

In dit artikel wordt ervan uitgegaan dat u een werkende basiskennis hebben van Tomcat- en Linux.  

## <a name="phase-1-create-an-image"></a>Fase 1: Een installatiekopie maken
In deze fase maakt u een virtuele machine met behulp van een Linux-installatiekopie in Azure.  

### <a name="step-1-generate-an-ssh-authentication-key"></a>Stap 1: Een SSH-sleutel voor verificatie genereren
SSH is een belangrijk hulpprogramma voor systeembeheerders. Configureren van de toegangsbeveiliging op basis van een wachtwoord human bepaald wordt echter niet aanbevolen. Kwaadwillende gebruikers kunnen in uw systeem op basis van een gebruikersnaam en een zwak wachtwoord verbreken.

Goed nieuws, is er een manier om te laat RAS geopend en u geen zorgen over wachtwoorden. Deze methode bestaat van verificatie met asymmetrische cryptografie. De persoonlijke sleutel van de gebruiker is het adres waarmee de verificatie wordt verleend. U kunt zelfs de gebruikersaccount niet toestaat wachtwoordverificatie vergrendelen.

Een ander voordeel van deze methode is dat u hoeft niet verschillende wachtwoorden aan te melden bij verschillende servers. U kunt verifiëren met behulp van de persoonlijke persoonlijke sleutel op alle servers, waarmee wordt voorkomen dat u hoeven onthouden verschillende wachtwoorden.



Volg deze stappen voor het genereren van de SSH-sleutel voor verificatie.

1. Download en installeer PuTTYgen van de volgende locatie: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2. Puttygen.exe worden uitgevoerd.
3. Klik op **genereren** om de sleutels te genereren. In het proces kunt u aanvraaggrootte verhogen door de muis over de leeg gebied in het venster te verplaatsen.  
   ![PuTTY sleutel Generator schermafbeelding van de nieuwe sleutel knop activeringscode genereren][1]
4. Nadat het proces genereren weergegeven Puttygen.exe uw nieuwe openbare sleutel.  
   ![PuTTY sleutel Generator schermafbeelding van de nieuwe openbare sleutel en het opslaan persoonlijke sleutel knop][2]
5. Selecteer en kopieer de openbare sleutel opslaan in een bestand met de naam publicKey.pem. Klik niet op **openbare sleutel opslaan**, omdat de indeling van de opgeslagen openbare sleutel verschilt van de openbare sleutel die we willen.
6. Klik op **persoonlijke sleutel opslaan**, en opslaan in een bestand met de naam privateKey.ppk.

### <a name="step-2-create-the-image-in-the-azure-portal"></a>Stap 2: De afbeelding in de Azure portal maken
1. In de [portal](https://portal.azure.com/), klikt u op **nieuw** in de taakbalk om een installatiekopie te maken. Kies de Linux-afbeelding die is gebaseerd op uw behoeften. Het volgende voorbeeld wordt de installatiekopie van het Ubuntu 14.04.
![Schermafbeelding van de portal die de nieuwe knop wordt weergegeven][3]

2. Voor **hostnaam**, geef de naam op voor de URL die u en de Internet-clients gebruiken voor toegang tot deze virtuele machine. Het laatste deel van de DNS-naam, bijvoorbeeld tomcatdemo definiëren. Azure wordt vervolgens de URL gegenereerd als tomcatdemo.cloudapp.net.  

3. Voor **SSH verificatiesleutel**, waarde van de sleutel van het bestand publicKey.pem, waarin de openbare sleutel die is gegenereerd door PuTTYgen kopiëren.  
![SSH-verificatiesleutel vak in de portal][4]

4. Andere instellingen naar wens configureren en klik vervolgens op **maken**.  

## <a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Fase 2: Uw virtuele machine voorbereiden voor Tomcat7
In deze fase wordt u een eindpunt configureert voor Tomcat-verkeer en maak verbinding met uw nieuwe virtuele machine.

### <a name="step-1-open-the-http-port-to-allow-web-access"></a>Stap 1: Open de HTTP-poort voor webtoegang
Eindpunten in Azure bestaan uit een protocol TCP of UDP, samen met een openbare en particuliere poort. De particuliere poort is de poort waarnaar de service op de virtuele machine luistert. De openbare poort is de poort die de Azure-cloudservice naar extern voor binnenkomend verkeer op basis van het Internet luistert.  

TCP-poort 8080 is het standaardpoortnummer dat Tomcat wordt geluisterd. Als deze poort wordt geopend met een Azure-eindpunt, u en andere internetclients hebben toegang tot Tomcat-pagina's.  

1. Klik in de portal op **Bladeren** > **virtuele machines**, en klik vervolgens op de virtuele machine die u hebt gemaakt.  
   ![Schermafbeelding van de map virtuele machines][5]
2. Een eindpunt toevoegen aan uw virtuele machine: klik op de **eindpunten** vak.
   ![Schermafbeelding van het vak eindpunten][6]
3. Klik op **Add**.  

   1. Voer een naam voor het eindpunt in voor het eindpunt **eindpunt**, en voer dan 80 in **openbare poort**.  

      Als u deze op 80 instellen, moet u niet het poortnummer in de URL die wordt gebruikt voor toegang tot Tomcat opnemen. Bijvoorbeeld: http://tomcatdemo.cloudapp.net.    

      Als u deze op een andere waarde, zoals 81 instellen, moet u het poortnummer toevoegen aan de URL voor toegang tot Tomcat. Bijvoorbeeld: http://tomcatdemo.cloudapp.net:81 /.
   2. Voer 8080 in **particuliere poort**. Standaard luistert Tomcat op TCP-poort 8080. Als u de standaardwaarde gewijzigd van Tomcat-poort luistert, moet u bijwerken **particuliere poort** moet hetzelfde zijn als de Tomcat poort luistert.  
      ![Schermopname van gebruikersinterface waarin de opdracht, de openbare poort en het particuliere poort toevoegen][7]
4. Klik op **OK** het eindpunt toevoegen aan uw virtuele machine.

### <a name="step-2-connect-to-the-image-you-created"></a>Stap 2: Verbinding maken met de installatiekopie die u hebt gemaakt
U kunt een SSH-hulpprogramma verbinding maken met uw virtuele machine. In dit voorbeeld gebruiken we PuTTY.  

1. De DNS-naam van uw virtuele machine ophalen van de portal.
    1. Klik op **Bladeren** > **virtuele machines**.
    2. Selecteer de naam van de virtuele machine en klik vervolgens op **eigenschappen**.
    3. In de **eigenschappen** tegel, kijkt u in de **domeinnaam** vak om op te halen van de DNS-naam.  

2. Ophalen van het poortnummer voor SSH-verbindingen van de **SSH** vak.  
![Schermafbeelding van het poortnummer voor SSH-verbinding][8]

3. Download [PuTTY](http://www.putty.org/).  

4. Nadat u hebt gedownload, klikt u op het uitvoerbare bestand Putty.exe. In de PuTTY-configuratie, de basisopties configureren met de hostnaam en het poortnummer dat is verkregen van de eigenschappen van de virtuele machine.   
![Schermafbeelding van de configuratie van de PuTTY naam en het poortnummer opties host][9]

5. Klik in het linkerdeelvenster op **verbinding** > **SSH** > **Auth**, en klik vervolgens op **Bladeren** om de locatie van het bestand privateKey.ppk te geven. Het bestand privateKey.ppk bevat de persoonlijke sleutel die wordt gegenereerd door PuTTYgen eerder in de ' stap 1: een installatiekopie maken ' sectie van dit artikel.  
![Schermafbeelding van de verbinding directory-hiërarchie en de knop Bladeren.][10]

6. Klik op **Open**. U wordt mogelijk door een berichtvenster gewaarschuwd. Als u de DNS-naam hebt geconfigureerd en het poortnummer juist, klikt u op **Ja**.
![Schermafbeelding van de melding][11]

7. U wordt gevraagd uw gebruikersnaam invoeren.  
![Schermopname die laat waar zien u de gebruikersnaam invoeren][12]

8. Geef de gebruikersnaam die u gebruikt voor het maken van de virtuele machine in de ' stap 1: een installatiekopie maken ' eerder in dit artikel. U ziet ongeveer als volgt:  
![Schermafbeelding van de verificatie-bevestiging][13]

## <a name="phase-3-install-software"></a>Fase 3: Installeren van software
In deze fase installeert u Java runtime environment, Tomcat7 en andere onderdelen Tomcat7.  

### <a name="java-runtime-environment"></a>Java runtime-omgeving
Tomcat is geschreven in Java. Er zijn twee soorten Java Development Kit (JDKs), OpenJDK en Oracle JDK. U kunt kiezen die u wilt.  

> [!NOTE]
> Beide JDKs hebben bijna de dezelfde code voor de klassen in de Java-API, maar de code voor de virtuele machine verschilt. OpenJDK doorgaans open bibliotheken gebruiken terwijl Oracle JDK doorgaans gesloten die zijn gebruikt. Oracle JDK heeft meer klassen en enkele fouten opgelost en Oracle JDK stabieler dan OpenJDK is.

#### <a name="install-openjdk"></a>OpenJDK installeren  

Gebruik de volgende opdracht voor het downloaden van OpenJDK.   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  


* Een map bevat de JDK-bestanden maken:  

        sudo mkdir /usr/lib/jvm  
* De JDK-bestanden uitpakken naar de map/usr/lib/jvm /:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/

#### <a name="install-oracle-jdk"></a>Oracle JDK installeren


Gebruik de volgende opdracht voor het downloaden van Oracle JDK van de Oracle-website.  

     wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  
* Een map bevat de JDK-bestanden maken:  

        sudo mkdir /usr/lib/jvm  
* De JDK-bestanden uitpakken naar de map/usr/lib/jvm /:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  
* Oracle JDK als de virtuele machine die standaard Java instellen:  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  

        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

#### <a name="confirm-that-java-installation-is-successful"></a>Bevestig dat de Java-installatie geslaagd is
U kunt een opdracht als volgt gebruiken om te controleren of Java runtime environment juist is geïnstalleerd:  

    java -version  

Als u OpenJDK hebt geïnstalleerd, ziet u een bericht als volgt: ![geslaagde OpenJDK installatie bericht][14]

Als u Oracle JDK hebt geïnstalleerd, ziet u een bericht als volgt: ![geslaagde Oracle JDK installatie bericht][15]

### <a name="install-tomcat7"></a>Tomcat7 installeren
Gebruik de volgende opdracht Tomcat7 installeren.  

    sudo apt-get install tomcat7  

Als u niet Tomcat7 gebruikt, gebruikt u de juiste variatie van deze opdracht.  

#### <a name="confirm-that-tomcat7-installation-is-successful"></a>Bevestig dat Tomcat7 installatie geslaagd is
Controleert of de Tomcat7 is geïnstalleerd, bladert u naar de DNS-naam van de Tomcat-server. In dit artikel is de voorbeeld-URL http://tomcatexample.cloudapp.net/. Als u een bericht als volgt ziet, Tomcat7 juist is geïnstalleerd.
![Bericht Tomcat7 installatie is voltooid][16]

### <a name="install-other-tomcat7-components"></a>Andere onderdelen Tomcat7 installeren
Er zijn andere optionele Tomcat-onderdelen die u kunt installeren.  

Gebruik de **sudo apt cache zoeken tomcat7** opdracht om alle beschikbare onderdelen te zien. Gebruik de volgende opdrachten om sommige nuttig onderdelen te installeren.  

    sudo apt-get install tomcat7-admin      #admin web applications

    sudo apt-get install tomcat7-user         #tools to create user instances  

## <a name="phase-4-configure-tomcat7"></a>Fase 4: Tomcat7 configureren
In deze fase, moet u Tomcat beheren.

### <a name="start-and-stop-tomcat7"></a>Starten en stoppen Tomcat7
De server Tomcat7 wordt automatisch gestart wanneer u deze installeert. U kunt deze ook starten met de volgende opdracht:   

    sudo /etc/init.d/tomcat7 start

Tomcat7 stoppen:

    sudo /etc/init.d/tomcat7 stop

De status van Tomcat7 weergeven:

    sudo /etc/init.d/tomcat7 status

Tomcat-services opnieuw starten: 

    sudo /etc/init.d/tomcat7 restart

### <a name="tomcat7-administration"></a>Tomcat7 beheer
U kunt het configuratiebestand voor gebruikers van Tomcat voor het instellen van uw beheerdersreferenties bewerken. Gebruik de volgende opdracht:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Hier volgt een voorbeeld:  
![Schermafbeelding van de uitvoer van de sudo vi][17]  

> [!NOTE]
> Maak een sterk wachtwoord voor de gebruikersnaam van de beheerder.  

Start Tomcat7 services met de volgende opdracht om ervoor te zorgen dat de wijzigingen van kracht na het bewerken van dit bestand:  

    sudo /etc/init.d/tomcat7 restart  

Open uw browser en voer **http://<your tomcat server DNS name>manager/html** als de URL. De URL is voor het voorbeeld in dit artikel http://tomcatexample.cloudapp.net/manager/html.  

Nadat u verbinding maakt ziet er ongeveer als volgt:  
![Schermopname van Tomcat Web Application Manager][18]

## <a name="common-issues"></a>Algemene problemen
### <a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Geen toegang tot de virtuele machine met Tomcat en Moodle vanaf het Internet
#### <a name="symptom"></a>Symptoom  
  Tomcat wordt uitgevoerd, maar u kunt niet de standaardpagina Tomcat bekijken met uw browser.
#### <a name="possible-root-cause"></a>Mogelijke hoofdoorzaken   

  * De Tomcat listen-poort is niet hetzelfde zijn als de particuliere poort van de virtuele machine-eindpunt voor Tomcat-verkeer.  

     Controleer uw openbare poort en particuliere poort eindpuntinstellingen en zorg ervoor dat de particuliere poort is dat hetzelfde als de Tomcat poort luistert. Zie ' stap 1: een installatiekopie maken ' sectie van dit artikel voor instructies over het configureren van eindpunten voor uw virtuele machine.  

     Om te bepalen van de Tomcat listen-poort, /etc/httpd/conf/httpd.conf (Red Hat release) of /etc/tomcat7/server.xml (Debian-versie) te openen. Standaard is de Tomcat listen-poort 8080. Hier volgt een voorbeeld:  

        <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"   URIEncoding="UTF-8"            redirectPort="8443" />  

     Als u een virtuele machine zoals Debian of Ubuntu en u wijzigen van de standaard poort wilt van Tomcat luisteren (bijvoorbeeld 8081) gebruikt, moet u ook de poort voor het besturingssysteem te openen. Opent u het profiel:  

        sudo vi /etc/default/tomcat7  

     Vervolgens Opmerkingen bij de laatste regel en 'Nee' op 'Ja' niet wijzigen.  

        AUTHBIND=yes
  2. De firewall is uitgeschakeld door de listen-poort van Tomcat.

     U ziet alleen de Tomcat-standaardpagina van de lokale host. Het probleem is waarschijnlijk dat de poort die wordt geluisterd door Tomcat, door de firewall is geblokkeerd. U kunt het hulpprogramma w3m gebruiken om te bladeren op de webpagina. De volgende opdrachten w3m installeren en blader naar de standaardpagina Tomcat:  


        sudo yum installeren w3m w3m-img


        w3m http://localhost: 8080  
#### <a name="solution"></a>Oplossing

  * Als de Tomcat luisteren poort is niet hetzelfde zijn als de particuliere poort van het eindpunt voor het verkeer naar de virtuele machine, hoeft u de particuliere poort om dat hetzelfde zijn als de Tomcat luisteren op poort te wijzigen.   
  2. Als het probleem wordt veroorzaakt door een firewall/iptables, moet u de volgende regels toevoegen aan /etc/sysconfig/iptables. De tweede regel is alleen nodig voor https-verkeer:  

      -A -p tcp -m tcp--port 80 -j accepteren invoer

      -A -p tcp -m tcp--Port 443 -j accepteren invoer  

     > [!IMPORTANT]
     > Zorg ervoor dat de vorige regels boven alle regels in die toegang tot, zoals de volgende globaal zou beperken zijn geplaatst: - een invoer -j afwijzen--afwijzen-met icmp host verboden



Als u wilt laden de iptables, voer de volgende opdracht:

    service iptables restart

Dit is getest op CentOS 6.3.

### <a name="permission-denied-when-you-upload-project-files-to-varlibtomcat7webapps"></a>Toestemming geweigerd wanneer u de project-bestanden naar /var/lib/tomcat7/webapps uploaden /
#### <a name="symptom"></a>Symptoom
  Wanneer u een client SFTP (zoals FileZilla) verbinding maken met uw virtuele machine en Ga naar /var/lib/tomcat7/webapps/naar de site publiceert, verschijnt u een foutbericht ziet er als volgt:  

     status:    Listing directory /var/lib/tomcat7/webapps
     Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
     Error:    /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
     Error:    File transfer failed
#### <a name="possible-root-cause"></a>Mogelijke hoofdoorzaken
  U bent niet gemachtigd voor toegang tot de map /var/lib/tomcat7/webapps.  
#### <a name="solution"></a>Oplossing  
  U moet de machtiging ophalen uit het root-account. Kunt u het eigendom van de map van hoofdmap voor de gebruikersnaam die u hebt gebruikt bij het inrichten van de machine. Hier volgt een voorbeeld met de naam van de account azureuser:  

     sudo chown azureuser -R /var/lib/tomcat7/webapps

  De optie -R om toe te passen de machtigingen voor alle bestanden in een map te gebruiken.  

  Met deze opdracht werkt ook voor mappen. De optie -R wijzigt de machtigingen voor alle bestanden en mappen in de map. Hier volgt een voorbeeld:  

     sudo chown -R username:group directory  

  Deze opdracht wijzigt eigendom (gebruiker en groep) voor alle bestanden en mappen die zich in de map.  

  De volgende opdracht wordt alleen de machtiging van de map van de map wijzigt. De bestanden en mappen in de map worden niet gewijzigd.  

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

---
title: Instellen van een toepassing ontwikkelaars beheerd distributie (ADCD) in de IBM zD & T v1 | Microsoft Docs
description: Voer een IBM Z Development en testomgeving (zD & T)-omgeving op Azure Virtual Machines (VM's).
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: f8af19056a343abdbafcd4ead8b072330cb41fd9
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57192069"
---
# <a name="set-up-an-application-developers-controlled-distribution-adcd-in-ibm-zdt-v1"></a>Instellen van een toepassing ontwikkelaars beheerd distributie (ADCD) in de IBM zD & T v1

U kunt een testomgeving (zD & T)-omgeving en IBM Z Development uitvoeren op Azure Virtual Machines (VM's). Deze omgeving wordt de architectuur voor IBM Z Series geëmuleerd. Tal van Z reeks besturingssystemen of -installaties (ook wel Z-exemplaren of -pakketten), die beschikbaar worden gesteld via aangepaste bundels de IBM toepassing ontwikkelaars beheerd distributies (ADCDs) met de naam kan hosten.

Dit artikel leest u hoe het instellen van een ADCD-instantie in een zD & T-omgeving op Azure. ADCDs maken voltooid Z reeks besturingssysteem implementaties voor ontwikkeling en testomgevingen die worden uitgevoerd in zD & T.

Zoals zD & T, ADCDs zijn alleen beschikbaar voor IBM-klanten en partners en zijn uitsluitend bedoeld voor ontwikkelings- en testdoeleinden. Ze zijn niet moet worden gebruikt voor productie-omgevingen. Talloze IBM-installatiepakketten kunnen worden gedownload via [Passport voordeel](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.guide.adcd.doc/topics/installation_ps.html) of [IBM PartnerWorld](https://www-356.ibm.com/partnerworld/wps/servlet/ContentHandler/isv_com_sys_zos_adcd).

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

- De [zD & T omgeving] [ ibm-install-z] eerder hebt ingesteld op Azure. In dit artikel wordt ervan uitgegaan dat u van de dezelfde Ubuntu 16.04 VM-installatiekopie eerder hebt gemaakt gebruikmaakt.

- Toegang tot de media ADCD via IBM PartnerWorld of Passport-voordeel.

- Een [licentieserver](https://www.ibm.com/support/knowledgecenter/en/SSTQBD_12.0.0/com.ibm.zsys.rdt.tools.user.guide.doc/topics/zdt_ee.html). Dit is vereist voor IBM zD & t uitvoeren De manier waarop u dit hebt gemaakt, is afhankelijk van hoe u de software van IBM licentie:

  - **Op basis van hardware licentieserver** vereist een USB-apparaat dat de rationele Tokens die nodig zijn voor toegang tot alle onderdelen van de software bevat. U moet dit verkrijgen van IBM.

  - **Op basis van software licentieserver** , moet u voor het instellen van een centrale server voor het beheer van de licentieverlening sleutels. Deze methode heeft de voorkeur en moet u voor het instellen van de sleutels die u van IBM in de beheerserver ontvangt.

## <a name="download-the-installation-packages-from-passport-advantage"></a>De installatiepakketten downloaden van de Passport-voordeel

Toegang tot de media ADCD is vereist. De onderstaande stappen wordt ervan uitgegaan dat u een IBM-klanten en kan profiteren van de Passport gebruiken. IBM partners kunt [IBM PartnerWorld](https://www-356.ibm.com/partnerworld/wps/servlet/ContentHandler/isv_com_sys_zos_adcd).

> [!NOTE]
> In dit artikel wordt ervan uitgegaan dat een Windows-PC wordt gebruikt voor toegang tot Azure portal en voor het downloaden van de IBM-media. Als u van een bureaublad voor Mac- of Ubuntu gebruikmaakt, kunnen de opdrachten en het proces voor het verkrijgen van de IBM-media enigszins verschillen.

1. Meld u aan bij [Passport voordeel](https://www.ibm.com/software/howtobuy/passportadvantage/paocustomer).

2. Selecteer **softwaredownloads** en **Media toegang**.

3. Selecteer **programma-aanbieding en het overeenkomst**, en klikt u op **doorgaan**.

4. Voer de beschrijving van het onderdeel of het onderdeelnummer en klikt u op **Finder**.

5. Klik desgewenst op de lijst alfabetische volgorde weergeven en theproduct weergeven met de naam.

6. Selecteer **alle besturingssystemen** in de **besturingssysteem veld**, en **alle talen** in de **talen veld**. Klik vervolgens op **gaat**.

7. Klik op **afzonderlijke bestanden selecteren** aan de lijst uitvouwen en weergeven van de afzonderlijke media om te downloaden.

8. Controleer of de pakketten die u wilt downloaden, selecteert u **downloaden**, en download de bestanden naar de map die u wilt.

## <a name="upload-the-adcd-packages"></a>De ADCD en weer andere uploaden

Nu dat u de pakketten hebt, moet u ze uploaden naar uw VM op Azure.

1. In de Azure-portal starten een **ssh** sessie met de Ubuntu-VM die u hebt gemaakt. Ga naar uw virtuele machine, selecteer de **overzicht** blade, en selecteer vervolgens **Connect**.

2. Selecteer de **SSH** tabblad en kopieer vervolgens de ssh-opdracht naar het Klembord.

3. Meld u aan bij uw virtuele machine met uw referenties en de [SSH-client](/azure/virtual-machines/linux/use-remote-desktop) keuze. In deze demo maakt gebruik van de Linux-uitbreidingen voor Windows 10, die wordt toegevoegd een bash-shell naar de opdrachtprompt van Windows. PuTTY net zo goed werkt.

4. Wanneer u bent aangemeld, maakt u een map voor het uploaden van de IBM-pakketten. Houd er rekening mee Linux is hoofdlettergevoelig. Bijvoorbeeld: voor deze demo wordt aangenomen dat de pakketten worden geüpload naar:

        /home/MyUserID/ZDT/adcd/nov2017/volumes

5. De bestanden met behulp van een SSH-client, zoals uploaden[WinSCP](https://winscp.net/eng/index.php). Omdat het SCP is een onderdeel van SSH, gebruikt deze poort 22, dit is wat SSH wordt gebruikt. Als uw lokale computer geen Windows is, typt u de [scp-opdracht](http://man7.org/linux/man-pages/man1/scp.1.html) in de SSH-sessie.

6. Voor het uploaden naar de virtuele machine van Azure-map die u hebt gemaakt, wordt de installatiekopie van opslag voor zD & T. initiëren

    > [!NOTE]
    > Zorg ervoor dat **ADCDTOOLS. XML** is opgenomen in voor het uploaden naar de **home/MyUserID/ZDT/adcd/nov2017** directory. U hebt dit later nodig.

7. Wacht tot de bestanden te uploaden, die het duurt enige tijd, afhankelijk van de verbinding naar Azure.

8. Wanneer de uploads voltooid zijn, gaat u naar de map volumes en decomprimeren van alle van de **gz** volumes:

    ```
        gunzip \*.gz
    ```
    
![Met Verkenner gedecomprimeerd gz volumes](media/01-gunzip.png)

## <a name="configure-the-image-storage"></a>De installatiekopie-opslag configureren

De volgende stap is het configureren van zD & n naar de geüploade pakket(ten) gebruiken. De installatiekopie van de opslag in een zD & T kunt u koppelen en de afbeeldingen gebruiken. Het kan SSH- of FTP gebruiken.

1. Start de **zDTServer**. Om dit te doen, moet u op het hoogste niveau. Voer de volgende twee opdrachten in volgorde:
    ```
        sudo su -
        /opt/ibm/zDT/bin/startServer
    ```
2. Houd er rekening mee de URL-uitvoer met de opdracht en deze URL gebruiken voor toegang tot de webserver. Het lijkt op:
     > https://(Your VM name or IP Address):9443/ZDTMC/index.HTML
     >
     > Vergeet niet dat de web access maakt gebruik van poort 9443. Gebruik deze optie om aan te melden met de webserver. De gebruikers-id voor ZD & T **zdtadmin** en het wachtwoord is **wachtwoord**.

    ![IBM zD&T Enterprise Edition Welcome screen](media/02-welcome.png)

3. Op de **Quick Start** pagina onder **configureren**, selecteer **Afbeeldingopslag**.

     ![IBM zD&T Enterprise Edition Quick Start screen](media/03-quickstart.png)

4. Op de **configureren afbeeldingopslag** weergeeft, schakelt **SSH File Transfer Protocol**.

5. Voor **hostnaam**, type **Localhost** en voer in het mappad voor waar u de installatiekopieën geüpload. Bijvoorbeeld: /home/MyUserID/ZDT/adcd/nov2017/volumes.

6. Voer de **gebruikers-ID** en **wachtwoord** voor de virtuele machine. Gebruik niet de ZD & T gebruikers-ID en het wachtwoord.

7. Test de verbinding om te controleren of u toegang hebt, en selecteer vervolgens **opslaan** aan de configuratie op te slaan.

## <a name="configure-the-target-environments"></a>Configureren van de doelomgevingen

De volgende stap is het configureren van de zD & T doelomgeving. Deze geëmuleerde gehoste omgeving is waar uw installatiekopieën worden uitgevoerd.

1. Op de **Quick Start** pagina onder **configureren**, selecteer **richten omgevingen**.

2. Op de **configureren doelomgevingen** weergeeft, schakelt **doel toevoegen**.

3. Selecteer **Linux**. IBM ondersteunt twee typen omgevingen, Linux en Cloud(OpenStack), maar deze demo op Linux wordt uitgevoerd.

4. Op de **toevoegen doelomgeving** pagina voor **hostnaam**, voer **localhost**. Houd **SSH-poort** ingesteld op **22**.

5. In de **doelomgeving label** voert u een label, zoals **MyCICS.**

     ![Omgeving doelscherm toevoegen](media/04-add-target.png)

## <a name="configure-adcd-and-deploy"></a>ADCD configureren en implementeren

Na het voltooien van de vorige configuratiestappen, moet u configureren zD & n naar de omgeving en weer andere en het doel te gebruiken. Nogmaals, kunt u de installatiekopie van de opslag in zD & T, zodat u kunt koppelen en gebruiken van de installatiekopieën gebruiken. Het kan SSH- of FTP gebruiken.

1. Op de **Quick Start** pagina onder **configureren**, selecteer **ADCD**. Een set met instructies worden weergegeven, waarin u de stappen die worden uitgevoerd moeten voordat een pakket ADCD kan worden gekoppeld. Dit verklaart waarom we met de naam de doelmap de manier waarop die we eerder hebben gedaan.

2. Ervan uitgaande dat alle installatiekopieën zijn geüpload naar de juiste mappen, klikt u op de **afbeelding van ADCD** koppeling die wordt weergegeven in de rechterbenedenhoek (weergegeven in stap 7 in de volgende schermafbeelding).

     ![IBM zD & T Enterprise Edition - scherm ADCD configureren](media/05-adcd.png)

## <a name="create-the-image"></a>De installatiekopie maken

Als de vorige stap in de configuratie voltooid is, de **maken van een installatiekopie met behulp van ADCD onderdelen** pagina wordt weergegeven.

1. Selecteer het volume (november 2017 in dit geval) om de verschillende pakketten die zich in het volume weer te geven.

2. Selecteer voor deze demo **klant informatie besturingselement System (CICS) - 5.3**.

3. In de **installatiekopienaam** typt u een naam voor de installatiekopie, zoals **MyCICS installatiekopie**.

4. Selecteer de **afbeelding maken** knop in de rechterbenedenhoek.

     ![IBM zD & T Enterprise Edition - maken van een installatiekopie met behulp van het scherm ADCD onderdelen](media/06-adcd.png)

5. In het venster dat wordt weergegeven, waarin u de installatiekopie is geïmplementeerd, kiest u **installatiekopieën**.

6. Op de **een installatiekopie implementeert op een doelomgeving** pagina, selecteert u de installatiekopie die u hebt gemaakt in de vorige pagina (**MyCICS installatiekopie**) en de doelomgeving eerder hebt gemaakt (**MyCICS**).

7. Geef uw referenties voor de virtuele machine (dat wil zeggen, niet de ztadmin referentie) in het volgende scherm.

8. Voer in het deelvenster met eigenschappen van het aantal **centrale processors (CPs)**, de hoeveelheid **systeemgeheugen (GB)**, en de **Implementatiemap** voor de installatiekopie die wordt uitgevoerd. Omdat dit een demo, houd u klein.

9. Zorg ervoor dat het selectievakje is ingeschakeld voor **automatisch probleem IPL opdracht z/OS na implementeren**.

     ![Eigenschappen van scherm](media/07-properties.png)

10. Selecteer **volledige**.

11. Selecteer **installatiekopie implementeren** uit de **een installatiekopie implementeert op een doelomgeving** pagina.

Uw installatiekopie nu implementeren en is gereed om te worden gekoppeld door een terminal emulator 3270.

> [!NOTE]
> Als u een foutbericht dat u hebt niet voldoende schijfruimte beschikbaar is ontvangt, houd er rekening mee dat de regio 151 Gb vereist.

Gefeliciteerd! U worden nu een IBM mainframe-omgeving in Azure uitgevoerd.

## <a name="learn-more"></a>Meer informatie

- [Mainframe-migratie: misverstanden en feiten](https://docs.microsoft.com/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/myths-and-facts)
- [IBM DB2 pureScale op Azure](https://docs.microsoft.com/azure/virtual-machines/linux/ibm-db2-purescale-azure)
- [Problemen oplossen](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/)
- [Mainframe ontrafelen aan Azure-migratie](https://azure.microsoft.com/en-us/resources/demystifying-mainframe-to-azure-migration/)

<!-- INTERNAL LINKS -->
[microfocus-get-started]: /microfocus/get-started.md
[microfocus-setup]: /microfocus/set-up-micro-focus-on-azure.md
[microfocus-demo]: /microfocus/demo.md
[ibm-get-started]: /ibm/get-started.md
[ibm-install-z]: install-ibm-z-environment.md
[ibm-demo]: /ibm/demo.md

---
title: Micro Focus Enterprise Server 4.0 en Enterprise Developer 4.0 installeren op Azure | Microsoft Docs
description: Opnieuw hosten van uw IBM z/OS mainframe-workloads met behulp van de Focus Micro-ontwikkeling en testen op Azure virtual machines (VM's).
services: virtual-machines-linux
documentationcenter: ''
author: njray
manager: edprice
editor: edprice
tags: ''
keywords: ''
ms.openlocfilehash: 33d0baf10df1882baf212c3e2c2683c8ca072fcc
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896437"
---
# <a name="install-micro-focus-enterprise-server-40-and-enterprise-developer-40-on-azure"></a>Micro Focus Enterprise Server 4.0 en Enterprise Developer 4.0 installeren op Azure

In dit artikel laat zien hoe het instellen van [Micro Focus Enterprise Server 4.0](https://www.microfocus.com/documentation/enterprise-developer/es30/) en [Micro Focus Enterprise Developer 4.0](https://www.microfocus.com/documentation/enterprise-developer/ed_30/) op Azure.

Een algemene workload op Azure is een ontwikkelings- en -omgeving, omdat het is dus voordelig geprijsd en eenvoudig te implementeren en te verbreken. Met Enterprise-Server heeft Micro Focus een van de grootste mainframe rehosting platformen beschikbaar gemaakt. U kunt z/OS-workloads uitvoeren op een goedkopere x86 platform op Azure met behulp van Windows of Linux-machines (VM's).

Deze instelling maakt gebruik van Azure-VM's waarop de installatiekopie van het Windows Server 2016 wordt uitgevoerd vanuit Azure Marketplace met Microsoft SQL Server 2017 is al geïnstalleerd. Deze instelling geldt ook voor Azure Stack.

De bijbehorende ontwikkelomgeving voor Enterprise-Server is Enterprise-ontwikkelaar, die wordt uitgevoerd op een van beide Microsoft Visual Studio 2017, Visual Studio Community (gratis te downloaden), of Eclipse. In dit artikel laat zien hoe om te implementeren met behulp van een Windows Server 2016 virtuele machine die wordt geleverd met Visual Studio 2017 is geïnstalleerd.

## <a name="prerequisites"></a>Vereisten

Voordat u aan de slag, bekijkt u deze vereisten voldoet:

- Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

- De Focus Micro-software en een geldige licentie of proeflicentie. Als u een bestaande Micro Focus-klant bent, moet u contact op met de vertegenwoordiger van uw Micro Focus. Anders [vraag een proefversie](https://www.microfocus.com/products/enterprise-suite/enterprise-server/trial/).

- Download de documentatie voor [Enterprise Server en Enterprise Developer](https://www.microfocus.com/documentation/enterprise-developer/#").

> [!NOTE]
> Er is een best practice voor het instellen van een site-naar-site virtueel particulier netwerk (VPN)-tunnel of een jumpbox, zodat u toegang tot de Azure VM's kunt beheren.

## <a name="install-enterprise-server"></a>Enterprise-Server installeren

1. Voor betere beveiliging en beheerbaarheid, houd rekening met het maken van een nieuwe resourcegroep voor dit project, bijvoorbeeld **RGMicroFocusEntServer**. Gebruik het eerste deel van de naam in Azure om het type resource zodat u gemakkelijk te herkennen in een lijst toe te wijzen.

2. Hiermee maakt u een virtuele machine. Selecteer in de Azure Marketplace, de virtuele machine en het besturingssysteem die u wilt. Hier volgt een aanbevolen instellingen:

    - **Enterprise Server**: Selecteer ES2 v3 VM (met 2 vcpu's en 16 GB geheugen) met Windows Server 2016 en SQL Server 2017 is geïnstalleerd. Deze installatiekopie is beschikbaar via de Azure Marketplace. Enterprise-Server kunt en Azure SQL Database gebruiken.

    - **Enterprise Developer**: Selecteer B2ms VM (met 2 vcpu's en 8 GB geheugen) met Windows 10 en Visual Studio is geïnstalleerd. Deze installatiekopie is beschikbaar via de Azure Marketplace.

3. Op de **basisbeginselen** blade, Voer uw gebruikersnaam en wachtwoord. Selecteer de **abonnement** en **locatie/de regio** u wilt gebruiken voor de virtuele machines. Selecteer **RGMicroFocusEntServer** voor de resourcegroep.

4. Beide VM's in hetzelfde virtuele netwerk plaatsen zodat ze met elkaar kunnen communiceren.

5. Accepteer de standaardwaarden voor de rest van de instellingen. Vergeet niet de gebruikersnaam en het wachtwoord die u voor de beheerder van deze virtuele machines maken.

6. Als de virtuele machines zijn gemaakt, opent u poorten voor inkomend verkeer 9003, 86 en 80 voor HTTP en 3389 voor RDP in op de Enterprise-Server-machine en 3389 op de computer van de ontwikkelaar.

7. Selecteer de ES2 v3-VM voor aanmelding bij de Enterprise-Server virtuele machine in Azure-portal. Ga naar de **overzicht** blade en selecteer **Connect** een RDP-sessie starten. Meld u aan met de referenties die u hebt gemaakt voor de virtuele machine.

8. Laden van de RDP-sessie de volgende twee bestanden. Aangezien dit Windows, zodat u kunt slepen en neerzetten van de bestanden naar de RDP-sessie:

    - **ES\_40. exe**, het installatiebestand van de Enterprise-Server.

    - **mflic**, het bijbehorende licentiebestand: Enterprise-Server niet laden zonder deze.

9. Dubbelklik op het bestand om de installatie te starten. In het eerste venster de installatielocatie selecteren en accepteer de gebruiksrechtovereenkomst.

     ![Micro Focus Enterprise Server Setup-scherm](media/01-enterprise-server.png)

     Wanneer de installatie is voltooid, wordt het volgende bericht weergegeven:

     ![Micro Focus Enterprise Server Setup-scherm](media/02-enterprise-server.png)

### <a name="check-for-updates"></a>Controleren op updates

Na de installatie moet controleren of er aanvullende updates sinds een aantal vereisten, zoals de Microsoft C++ Redistributable en .NET Framework zijn geïnstalleerd, samen met Enterprise-Server.

### <a name="upload-the-license"></a>De licentie uploaden

1. Start het beheer van de licentie Micro Focus.

2. Klik op **Start** \> **Micro Focus License Manager** \> **gebruiksrechtovereenkomst beheerprogramma**, en klik vervolgens op de **installeren** tabblad. Kies het type licentie-indeling te uploaden: een licentiebestand of de licentiecode van een voor 16 tekens. Bijvoorbeeld, voor een bestand in **licentiebestand**, blader naar de **mflic** bestand eerder hebt geüpload naar de virtuele machine en selecteer **licenties installeren**.

     ![In het dialoogvenster van micro Focus gebruiksrechtovereenkomst beheerprogramma](media/03-enterprise-server.png)

3. Controleer of dat Enterprise-Server wordt geladen. Probeer de beheersite Enterprise Server vanuit een browser met behulp van deze URL te starten <http://localhost:86/> . De pagina beheer van Enterprise-Server wordt weergegeven, zoals wordt weergegeven.

     ![De pagina beheer van Enterprise-Server](media/04-enterprise-admin.png)

## <a name="install-enterprise-developer-on-the-developer-machine"></a>Enterprise Developer installeren op de machine voor ontwikkelaars

1. Selecteer de resourcegroep waarin u eerder hebt gemaakt (bijvoorbeeld **RGMicroFocusEntServer**), selecteer vervolgens de installatiekopie van de ontwikkelaar.

2. Als u wilt aanmelden bij de virtuele machine, gaat u naar de **overzicht** blade en selecteer **Connect**. Hiermee wordt een RDP-sessie gestart. Meld u aan met de referenties die u hebt gemaakt voor de virtuele machine.

3. Laden van de RDP-sessie de volgende twee bestanden (slepen en neerzetten als u):

    - **edvs2017.exe**, het installatiebestand van de Enterprise-Server.

    - **mflic**, het bijbehorende licentiebestand (Enterprise Developer wordt niet laden zonder deze).

4. Dubbelklik op de **edvs2017.exe** bestand om de installatie te starten. In het eerste venster de installatielocatie selecteren en accepteer de gebruiksrechtovereenkomst. Als u wilt, kiest u **installeren Rumba 9.5** voor het installeren van deze terminal emulator, u waarschijnlijk moet.

     ![Micro Focus Enterprise Developer voor dialoogvenster van de installatie van Visual Studio 2017](media/04-enterprise-server.png)

5. Nadat setup voltooid is, wordt het volgende bericht weergegeven:

     ![Bericht Setup is voltooid](media/05-enterprise-server.png)

6. Start de Micro Focus License Manager net zoals u dit hebt gedaan voor Enterprise-Server. Kies **Start** \> **Micro Focus License Manager** \> **gebruiksrechtovereenkomst beheerprogramma**, en klik op de **installeren**tabblad.

7. Kies het type licentie-indeling te uploaden: een licentiebestand of de licentiecode van een voor 16 tekens. Bijvoorbeeld, voor een bestand in **licentiebestand**, blader naar de **mflic** bestand eerder hebt geüpload naar de virtuele machine en selecteer **licenties installeren**.

     ![In het dialoogvenster van micro Focus gebruiksrechtovereenkomst beheerprogramma](/edia/07-enterprise-server.png)

Als ontwikkelaar wordt geladen, is uw implementatie van een omgeving met Micro Focus ontwikkelen en testen op Azure voltooid.

## <a name="next-steps"></a>Volgende stappen

- [De Bank Demo-toepassing instellen](./demo.md)
- [Enterprise-Server wordt uitgevoerd in Docker-containers](./run-enterprise-server-container.md)
- [Mainframe-toepassingen migreren](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/application-strategies)

---
title: Een Web-App publiceren naar een Azure virtuele machine vanuit Visual Studio | Microsoft Docs
description: Een ASP.NET-webtoepassing toepassing publiceren in Azure een virtuele Machine vanuit Visual Studio
services: virtual-machines-windows
documentationcenter: 
author:
- kraigb
- justcla
manager: ghogen
editor: 
tags: azure-service-management
ms.assetid: 70267837-3629-41e0-bb58-2167ac4932b3
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 11/03/2017
ms.author:
- kraigb
- justcla
ms.openlocfilehash: 74ac1f27620e4871c3c6972baa949c64d850fe4c
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="publish-an-aspnet-web-app-to-an-azure-vm-from-visual-studio"></a>Een ASP.NET-Web-App publiceren naar een Azure virtuele machine vanuit Visual Studio

Dit document wordt beschreven hoe u een ASP.NET-webtoepassing voor het gebruik van een virtuele Azure-machine (VM) publiceert de **Microsoft Azure Virtual Machines** publicatiefunctie in Visual Studio 2017.  

## <a name="prerequisites"></a>Vereisten
Om de Visual Studio gebruikt voor het publiceren van een ASP.NET-project naar een Azure-virtuele machine, de virtuele machine moet correct zijn ingesteld.

- Machine moet worden geconfigureerd voor het uitvoeren van een ASP.NET-webtoepassingen en Web Deploy geïnstalleerd hebben.

- De virtuele machine moet een DNS-naam zijn geconfigureerd. Zie voor meer informatie [een volledig gekwalificeerde domeinnaam in de Azure portal maken voor een virtuele machine van Windows](portal-create-fqdn.md).

## <a name="publish-your-aspnet-web-app-to-the-azure-vm-using-visual-studio"></a>Uw ASP.NET-web-app publiceren naar de virtuele machine in Azure met Visual Studio
De volgende sectie wordt beschreven hoe voor het publiceren van een bestaande ASP.NET-webtoepassing naar Azure een virtuele machine.

1. Open uw web-app-oplossing in Visual Studio 2017.
2. Met de rechtermuisknop op het project in Solution Explorer en kiest u **publiceren...**
3. Gebruik de pijl naar rechts van de pagina schuift de publicatieopties totdat u **Microsoft Azure Virtual Machines**.  

   ![Publiceren van pagina - pijl-rechts]

4. Selecteer de **Microsoft Azure Virtual Machines** pictogram en selecteer **publiceren**.

   ![Publiceren van pagina - pictogram van de virtuele Machine in Microsoft Azure]

5. Kies de juiste account (met de Azure-abonnement verbonden met uw virtuele machine).  
   - Als u bent aangemeld bij Visual Studio, wordt de lijst met accounts gevuld met alle geverifieerde accounts.  
   - Als u niet bent aangemeld, of als het account u moet niet wordt vermeld, kiest u '... een account toevoegen' en volg de aanwijzingen om aan te melden.  
   ![Azure-Account Selector]  

6. Selecteer de relevante VM in de lijst van bestaande virtuele Machines.

   > [!Note]
   > Vullen van deze lijst kan enige tijd duren.

   ![Azure VM-Selector]

7. Klik op OK om te beginnen met de publicatie.

8. Als u om referenties gevraagd, geeft u de gebruikersnaam en het wachtwoord van een gebruikersaccount op de doel-virtuele machine die is geconfigureerd met het publiceren van rechten (meestal de admin-gebruikersnaam en wachtwoord dat wordt gebruikt bij het maken van de virtuele machine).  

   ![Web Deploy-aanmelding]

9. Het beveiligingscertificaat accepteren.

   ![Certificaatfout]

10. Bekijk het uitvoervenster om te controleren van de voortgang van de bewerking uit te publiceren.

    ![Venster Output]

11. Als de publicatie is gelukt, wordt een browser gestart om de URL van de nieuwe site te openen.

**Success!**

U hebt nu uw web-app naar gepubliceerd Azure een virtuele machine.

## <a name="publish-page-options"></a>Paginaopties voor publiceren

Nadat de wizard Publiceren is voltooid, wordt de pagina publiceren geopend in het document goed met het nieuwe publicatieprofiel geselecteerd.

### <a name="re-publish"></a>Opnieuw publiceren

Om uw webtoepassing updates publiceert, selecteer de **publiceren** knop op de pagina publiceren.  
- Als u wordt gevraagd, gebruikersnaam en wachtwoord invoeren.  
- Publicatie begint onmiddellijk.

![Pagina publiceren: de knop publiceren]

### <a name="modify-publish-profile-settings"></a>Wijzig publish settings-profiel

Als u wilt weergeven en wijzigen van de publicatie-instellingen voor profiel, selecteer **instellingen...** .  

![Pagina - knop Instellingen publiceren]

Uw instellingen moeten er ongeveer als volgt uitzien:  

![Publicatie-instellingen - pagina-verbinding]

#### <a name="save-user-name-and-password"></a>Gebruikersnaam en wachtwoord opslaan
- U kunt vullen om te voorkomen verificatiegegevens geven telkens wanneer u publiceert, de **gebruikersnaam** en **wachtwoord** velden in en selecteer de **wachtwoord opslaan** vak.
- Gebruik de **Validate Connection** knop om te bevestigen dat u de juiste gegevens hebt ingevoerd.

#### <a name="deploy-to-clean-web-server"></a>Om op te ruimen webserver implementeren

- Als u wilt ervoor zorgen dat de webserver een schone kopie van de webtoepassing na elke uploaden (en dat er geen andere bestanden zijn verlaten van een eerdere implementatie verkeerd-om rond), kunt u controleren de **aanvullende bestanden op de bestemming verwijderen** selectievakje in de **instellingen** tabblad.

- Waarschuwing: In de publicatie met deze instelling worden alle bestanden die aanwezig zijn op de webserver (map wwwroot) verwijderd. Zorg ervoor dat u weet dat de status van de machine voordat u publiceert met deze optie is ingeschakeld. 

![Publicatie-instellingen - pagina-instellingen]

## <a name="next-steps"></a>Volgende stappen

### <a name="set-up-cicd-for-automated-deployment-to-azure-vm"></a>CI/CD instellen voor automatische implementatie naar Azure VM

Als u een pijplijn continue levering met Visual Studio Team Service instelt, Zie [implementeren naar een virtuele Windows-Machine](https://docs.microsoft.com/vsts/build-release/apps/cd/deploy-webdeploy-iis-deploygroups).

[VM Overview - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSName.png
[IP Address Config - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/IPAddressConfigDNSName.png
[VM Overview - DNS Configured]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSConfigured.png
[Publiceren van pagina - pijl-rechts]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageRightArrow.png
[Publiceren van pagina - pictogram van de virtuele Machine in Microsoft Azure]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageMicrosoftAzureVirtualMachineIcon.png
[Azure-Account Selector]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectAccount.png
[Azure VM-Selector]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectVM.png
[Web Deploy-aanmelding]: ../../../includes/media/publish-web-app-from-visual-studio/WebDeployLogin.png
[Certificaatfout]: ../../../includes/media/publish-web-app-from-visual-studio/CertificateError.png
[Venster Output]: ../../../includes/media/publish-web-app-from-visual-studio/OutputWindow.png
[Pagina publiceren: de knop publiceren]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPagePublishButton.png
[Pagina - knop Instellingen publiceren]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageSettingsButton.png
[Publicatie-instellingen - pagina-verbinding]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsConnectionPage.png
[Publicatie-instellingen - pagina-instellingen]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsSettingsPage.png

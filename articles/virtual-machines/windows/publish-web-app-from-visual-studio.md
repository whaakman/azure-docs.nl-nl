---
title: Een Web-App publiceren naar een Azure-VM vanuit Visual Studio
description: Publiceren van een ASP.NET-webtoepassing met een Azure-Machine vanuit Visual Studio
services: virtual-machines-windows
author: ghogen
manager: douge
tags: azure-service-management
ms.assetid: 70267837-3629-41e0-bb58-2167ac4932b3
ms.prod: visual-studio-dev15
ms.technology: vs-azure
ms.custom: vs-azure
ms.workload: azure-vs
ms.topic: conceptual
ms.date: 11/03/2017
ms.author: ghogen
ms.openlocfilehash: d5fd6041f21700ffddd3b2c4755b7ce01691681e
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44304080"
---
# <a name="publish-an-aspnet-web-app-to-an-azure-vm-from-visual-studio"></a>Een ASP.NET-Web-App publiceren naar een Azure-VM vanuit Visual Studio

Dit document wordt beschreven hoe u voor het publiceren van een ASP.NET-webtoepassing op een virtuele Azure-machine (VM) met behulp van de **Microsoft Azure Virtual Machines** publiceerfunctie in Visual Studio 2017.  

## <a name="prerequisites"></a>Vereiste onderdelen
Gebruik Visual Studio voor het publiceren van een ASP.NET-project met een Azure VM is de virtuele machine moet correct zijn ingesteld.

- Machine moet worden geconfigureerd voor het uitvoeren van een ASP.NET-webtoepassing en Web Deploy geïnstalleerd hebben.

- De virtuele machine moet een DNS-naam geconfigureerd. Zie voor meer informatie, [een volledig gekwalificeerde domeinnaam voor een Windows-VM maken in Azure portal](portal-create-fqdn.md).

## <a name="publish-your-aspnet-web-app-to-the-azure-vm-using-visual-studio"></a>Uw ASP.NET-web-app publiceren naar de Azure-VM met behulp van Visual Studio
De volgende sectie wordt beschreven hoe u een bestaande ASP.NET-webtoepassing met een Azure-machine publiceren.

1. Open de oplossing van uw web-app in Visual Studio 2017.
2. Met de rechtermuisknop op het project in Solution Explorer en kies **publiceren...**
3. Gebruik de pijl aan de rechterkant van de pagina schuift de publicatieopties totdat u **Microsoft Azure Virtual Machines**.  

   ![Pagina - pijl-rechts publiceren]

4. Selecteer de **Microsoft Azure Virtual Machines** pictogram en selecteer **publiceren**.

   ![Pagina - Microsoft Azure Virtual Machine-pictogram publiceren]

5. Kies het gewenste account (met Azure-abonnement verbonden met uw virtuele machine).  
   - Als u bent aangemeld bij Visual Studio, wordt de lijst met accounts wordt gevuld met alle geverifieerde accounts.  
   - Als u niet bent aangemeld, of als het account u moet niet wordt weergegeven, kiest u 'Add an account...' en volg de aanwijzingen om aan te melden.  
   ![Azure-Account selecteren]  

6. Selecteer de juiste virtuele machine in de lijst van de bestaande virtuele Machines.

   > [!Note]
   > Invullen van deze lijst kan enige tijd duren.

   ![Azure VM-Selector]

7. Klik op OK om het te publiceren.

8. Wanneer u hierom wordt gevraagd om referenties, geef de gebruikersnaam en het wachtwoord van een gebruikersaccount op de doel-VM die is geconfigureerd met het publiceren van rechten (doorgaans de admin-gebruikersnaam en wachtwoord dat wordt gebruikt bij het maken van de virtuele machine).  

   ![Web Deploy-aanmelding]

9. Het beveiligingscertificaat dat accepteren.

   ![Certificaatfout]

10. Bekijk het uitvoervenster om te controleren of de voortgang van de bewerking publiceren.

    ![Uitvoervenster]

11. Als de publicatie is voltooid, wordt een browser gestart om de URL van de zojuist gepubliceerde site te openen.

**Succes!**

U hebt nu uw web-app met een Azure-machine gepubliceerd.

## <a name="publish-page-options"></a>Opties voor pagina publiceren

Nadat de wizard Publiceren is voltooid, wordt de pagina publiceren in het document geopend goed met het nieuwe publicatieprofiel geselecteerd.

### <a name="re-publish"></a>Opnieuw publiceren

Voor het publiceren van updates voor uw web-App, selecteer de **publiceren** knop op de pagina publiceren.  
- Als u hierom wordt gevraagd, voert u gebruikersnaam en wachtwoord.  
- Publicatie begint onmiddellijk.

![Pagina publiceren: de knop publiceren]

### <a name="modify-publish-profile-settings"></a>Wijzigen publicatie-profielinstellingen

Als u wilt weergeven en wijzigen van de publicatie-instellingen voor profiel, selecteer **instellingen...** .  

![Pagina - knop Instellingen publiceren]

Uw instellingen moeten er ongeveer als volgt uit:  

![Publicatie-instellingen - pagina verbinding]

#### <a name="save-user-name-and-password"></a>Gebruikersnaam en wachtwoord opslaan
- Als u wilt voorkomen dat de verificatie-informatie geven, telkens wanneer u publiceren, u kunt vullen de **gebruikersnaam** en **wachtwoord** velden in en selecteer de **wachtwoord opslaan** vak.
- Gebruik de **verbinding valideren** knop om te bevestigen dat u de juiste informatie hebt ingevoerd.

#### <a name="deploy-to-clean-web-server"></a>Als u wilt opschonen webserver implementeren

- Als u ervoor zorgen wilt dat de webserver een nieuwe kopie van de web-App na elke uploaden heeft (en dat er geen andere bestanden worden gehandhaafd blijft hangen rond vanuit een eerdere implementatie), kunt u controleren de **aanvullende bestanden op de bestemming verwijderen** selectievakje in de **instellingen** tabblad.

- Waarschuwing: Publiceren met deze instelling verwijdert alle bestanden die aanwezig zijn op de webserver (map wwwroot). Zorg ervoor dat u weten wat de toestand van de machine voordat u publiceert met deze optie is ingeschakeld. 

![Publicatie-instellingen - pagina instellingen]

## <a name="next-steps"></a>Volgende stappen

### <a name="set-up-cicd-for-automated-deployment-to-azure-vm"></a>CI/CD instellen voor automatische implementatie voor Azure-VM

Als u een pipeline voor continue levering met Azure-pijplijnen instelt, Zie [implementeren met een Windows-VM](https://docs.microsoft.com/vsts/build-release/apps/cd/deploy-webdeploy-iis-deploygroups).

[VM Overview - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSName.png
[IP Address Config - DNS Name]: ../../../includes/media/publish-web-app-from-visual-studio/IPAddressConfigDNSName.png
[VM Overview - DNS Configured]: ../../../includes/media/publish-web-app-from-visual-studio/VMOverviewDNSConfigured.png
[Pagina - pijl-rechts publiceren]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageRightArrow.png
[Pagina - Microsoft Azure Virtual Machine-pictogram publiceren]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageMicrosoftAzureVirtualMachineIcon.png
[Azure-Account selecteren]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectAccount.png
[Azure VM-Selector]: ../../../includes/media/publish-web-app-from-visual-studio/ChooseVM-SelectVM.png
[Web Deploy-aanmelding]: ../../../includes/media/publish-web-app-from-visual-studio/WebDeployLogin.png
[Certificaatfout]: ../../../includes/media/publish-web-app-from-visual-studio/CertificateError.png
[Uitvoervenster]: ../../../includes/media/publish-web-app-from-visual-studio/OutputWindow.png
[Pagina publiceren: de knop publiceren]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPagePublishButton.png
[Pagina - knop Instellingen publiceren]: ../../../includes/media/publish-web-app-from-visual-studio/PublishPageSettingsButton.png
[Publicatie-instellingen - pagina verbinding]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsConnectionPage.png
[Publicatie-instellingen - pagina instellingen]: ../../../includes/media/publish-web-app-from-visual-studio/PublishSettingsSettingsPage.png

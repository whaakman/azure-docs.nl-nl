---
title: Functies die niet worden gestart oplossen | Microsoft Docs
description: Hier volgen enkele veelvoorkomende redenen waarom een cloudservicerol kan niet worden gestart. Oplossingen voor deze problemen worden ook gegeven.
services: cloud-services
documentationcenter: ''
author: simonxjx
manager: felixwu
editor: ''
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 06/15/2018
ms.author: v-six
ms.openlocfilehash: a73559e233f647d5bf0812a5acdf5e19f05b0858
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255418"
---
# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Problemen oplossen van cloudservicerollen die niet worden gestart
Hier volgen enkele veelvoorkomende problemen en oplossingen met betrekking tot Azure Cloud Services functies die niet worden gestart.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Ontbrekende dll's of afhankelijkheden
Niet-reagerende rollen en functies die zijn achterblijvende tussen **Bezig met initialiseren**, **bezet**, en **stoppen** Staten kunnen worden veroorzaakt door ontbrekende dll's of assembly's.

Symptomen van het ontbrekende dll's of assembly's zijn:

* Uw rolinstantie wordt doorlopen **Bezig met initialiseren**, **bezet**, en **stoppen** Staten.
* Uw rolinstantie is verplaatst naar **gereed** , maar als u naar uw web-App navigeren, de pagina niet wordt weergegeven.

Er zijn enkele aanbevolen methoden voor het onderzoeken van deze problemen.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Vaststellen van problemen met ontbrekende dll-bestand in een Webrol
Wanneer u gaat u naar een website die wordt geïmplementeerd in een web-rol en de browser een vergelijkbaar met de volgende serverfout wordt weergegeven, kan dit betekenen dat een DLL-bestand ontbreekt.

![Serverfout in '/' Application.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Problemen vaststellen door het uitschakelen van aangepaste fouten
Volledig foutgegevens kunnen worden bekeken door te configureren van het bestand web.config voor de Webrol in de aangepaste foutpagina modus instellen op uitgeschakeld en de service opnieuw te implementeren.

Gedetailleerde fouten weergeven zonder gebruik van extern bureaublad:

1. Open de oplossing in Microsoft Visual Studio.
2. In de **Solution Explorer**en zoek het bestand web.config en open deze.
3. In het bestand web.config, zoek de sectie system.web en voeg de volgende regel toe:

    ```xml
    <customErrors mode="Off" />
    ```
4. Sla het bestand op.
5. Verpakken en implementeer de service opnieuw.

Zodra de service opnieuw is geïmplementeerd, ziet u een foutbericht weergegeven met de naam van de ontbrekende assembly of DLL-bestand.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Problemen vaststellen door de fout op afstand weer te geven
U kunt Extern bureaublad gebruiken voor toegang tot de rol en gedetailleerde foutgegevens extern weergeven. Gebruik de volgende stappen uit om de fouten met behulp van extern bureaublad weer te geven:

1. Zorg ervoor dat de Azure SDK 1.3 of hoger is geïnstalleerd.
2. Tijdens de implementatie van de oplossing met behulp van Visual Studio, extern bureaublad inschakelen. Zie voor meer informatie, [extern bureaublad inschakelen voor een rol in Azure Cloud Services met behulp van Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md).
3. In de Microsoft Azure-portal, zodra het exemplaar de status van wordt **gereed**afstand verbinding met het exemplaar. Zie voor meer informatie over het gebruik van de extern bureaublad met Cloud Services, [afstand verbinding met de rolinstanties](cloud-services-role-enable-remote-desktop-new-portal.md#remote-into-role-instances).
5. Meld u aan de virtuele machine met behulp van de referenties die zijn opgegeven bij de configuratie van extern bureaublad.
6. Open een opdrachtvenster.
7. Typ `IPconfig`.
8. Noteer de waarde IPV4-adres.
9. Open Internet Explorer.
10. Typ het adres en de naam van de web-App. Bijvoorbeeld `http://<IPV4 Address>/default.aspx`.

Navigeren naar de website retourneert gedetailleerde foutberichten nu:

* Serverfout in '/' Application.
* Beschrijving: Er is een onverwerkte uitzondering opgetreden tijdens het uitvoeren van de huidige webaanvraag. Controleer de stack-trace voor meer informatie over de fout en waar deze afkomstig is in de code.
* Details van uitzondering: System.IO.FIleNotFoundException: kan niet worden geladen, bestand of de assembly ' Microsoft.WindowsAzure.StorageClient, versie 1.1.0.0, Culture = neutral, PublicKeyToken = 31bf856ad364e35 =' of een van de bijbehorende afhankelijkheden. Het systeem kan het opgegeven bestand niet vinden.

Bijvoorbeeld:

![Expliciete Serverfout in '/' Application](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Problemen met behulp van de rekenemulator
De Microsoft Azure-rekenemulator kunt u problemen vaststellen en oplossen van problemen met ontbrekende afhankelijkheden en fouten van web.config.

Voor de beste resultaten bij het gebruik van deze methode van de diagnose van, moet u een computer of virtuele machine waarvoor een schone installatie van Windows. Gebruik voor het beste de Azure-omgeving simuleren, Windows Server 2008 R2 x64.

1. Installeer de zelfstandige versie van de [Azure SDK](https://azure.microsoft.com/downloads/).
2. Bouw het cloudserviceproject op de ontwikkelcomputer.
3. Navigeer in Windows Verkenner naar de map bin\debug van het cloudserviceproject.
4. Kopieer het .csx map- en .cscfg-bestand naar de computer die u gebruikt om op te sporen van de problemen.
5. Open een venster van de Azure SDK-opdrachtprompt en typ op de machine schone `csrun.exe /devstore:start`.
6. Typ het volgende achter de opdrachtprompt `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.
7. Wanneer de rol wordt gestart, ziet u de gedetailleerde foutgegevens in Internet Explorer. U kunt ook de standaard Windows hulpprogramma's voor probleemoplossing gebruiken bij nader onderzoek het probleem.

## <a name="diagnose-issues-by-using-intellitrace"></a>Problemen met behulp van IntelliTrace
Voor werkrollen en webrollen die gebruikmaken van .NET Framework 4, kunt u [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), die beschikbaar is in Microsoft Visual Studio Enterprise.

Volg deze stappen voor het implementeren van de service met IntelliTrace ingeschakeld:

1. Bevestig dat de Azure SDK 1.3 of hoger is geïnstalleerd.
2. De oplossing implementeren met behulp van Visual Studio. Tijdens de implementatie, controleert u de **IntelliTrace inschakelen voor .NET 4 rollen** selectievakje.
3. Nadat het exemplaar wordt gestart, opent u de **Server Explorer**.
4. Vouw de **Azure\\Cloudservices** knooppunt en zoekt u de implementatie.
5. De implementatie uitbreiden tot u de rolinstanties. Met de rechtermuisknop op een van de exemplaren.
6. Kies **protokoly intellitrace weergave**. De **IntelliTrace samenvatting** wordt geopend.
7. Zoek de sectie uitzonderingen van de samenvatting openen. Als er uitzonderingen zijn, de sectie wordt worden gelabeld **uitzonderingsgegevens**.
8. Vouw de **uitzonderingsgegevens** en zoek naar **System.IO.FileNotFoundException** vergelijkbaar met de volgende fouten:

![Uitzonderingsgegevens, ontbrekende bestand of assembly](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Ontbrekende dll's en-assembly 's
Om ontbrekende dll-bestand en de assembly-fouten op te lossen, als volgt te werk:

1. Open de oplossing in Visual Studio.
2. In **Solution Explorer**, open de **verwijzingen** map.
3. Klik op de assembly die is geïdentificeerd in de fout.
4. In de **eigenschappen** deelvenster, zoek **lokale kopie van de eigenschap** en stel de waarde voor **waar**.
5. Implementeer de cloudservice opnieuw.

Nadat u hebt gecontroleerd dat alle fouten zijn opgelost, kunt u de service implementeren zonder te controleren of de **IntelliTrace inschakelen voor .NET 4 rollen** selectievakje.

## <a name="next-steps"></a>Volgende stappen
Meer weergeven [artikelen over probleemoplossing](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) voor cloudservices.

Zie voor informatie over het oplossen van problemen met de rol van de cloud service met behulp van Azure PaaS computer diagnostische gegevens, [van Kevin Williamson blogserie](https://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

---
title: Functies die niet worden gestart oplossen | Microsoft Docs
description: Hier volgen enkele veelvoorkomende redenen waarom een Service in de Cloud-rol kan niet worden gestart. Oplossingen voor deze problemen worden ook gegeven.
services: cloud-services
documentationcenter: 
author: simonxjx
manager: felixwu
editor: 
tags: top-support-issue
ms.assetid: 674b2faf-26d7-4f54-99ea-a9e02ef0eb2f
ms.service: cloud-services
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/03/2017
ms.author: v-six
ms.openlocfilehash: d24a55fd3b93760035f852a24537c1cec9e4bab2
ms.sourcegitcommit: cc03e42cffdec775515f489fa8e02edd35fd83dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2017
---
# <a name="troubleshoot-cloud-service-roles-that-fail-to-start"></a>Problemen met Cloud Service-functies die niet worden gestart
Hier volgen enkele veelvoorkomende problemen en oplossingen die zijn gerelateerd aan Azure Cloud Services functies die niet worden gestart.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="missing-dlls-or-dependencies"></a>Ontbrekende dll's of afhankelijkheden
Niet-reagerende rollen en functies die zijn functioneert tussen **Bezig met initialiseren**, **bezet**, en **stoppen** statussen kunnen zijn veroorzaakt door ontbrekende dll-bestanden of assembly's.

Problemen met ontbrekende dll's of assembly's zijn:

* De rolinstantie is doorlopen **Bezig met initialiseren**, **bezet**, en **stoppen** statussen.
* Uw rolexemplaar is verplaatst naar **gereed** maar als u uw webtoepassing navigeert, wordt de pagina wordt niet weergegeven.

Er zijn verschillende aanbevolen methoden om deze problemen te onderzoeken.

## <a name="diagnose-missing-dll-issues-in-a-web-role"></a>Vaststellen van problemen met ontbrekende dll-bestand in een Webrol
Wanneer u navigeren naar een website die wordt geïmplementeerd in een web-rol en de browser een vergelijkbaar met de volgende serverfout wordt weergegeven, kan dit wijzen op een DLL-bestand ontbreekt.

![Serverfout in '/' toepassing.](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503388.png)

## <a name="diagnose-issues-by-turning-off-custom-errors"></a>Problemen vaststellen door het uitschakelen van aangepaste fouten
Gedetailleerde foutinformatie kan worden weergegeven door het configureren van het bestand web.config voor de Webrol in de aangepaste fout-modus instellen op uitgeschakeld en opnieuw distribueren van de service.

Gedetailleerde fouten zonder gebruik van extern bureaublad weergeven:

1. Open de oplossing in Microsoft Visual Studio.
2. In de **Solution Explorer**en zoek het bestand web.config en open deze.
3. In het bestand web.config, zoek naar de sectie system.web en voeg de volgende regel:

    ```xml
    <customErrors mode="Off" />
    ```
4. Sla het bestand op.
5. Opnieuw verpakken en implementeer de service opnieuw.

Wanneer de service is geïmplementeerd, ziet u een foutbericht met de naam van de ontbrekende assembly- of DLL-bestand.

## <a name="diagnose-issues-by-viewing-the-error-remotely"></a>Problemen vaststellen door het bekijken van de fout op afstand
U kunt Extern bureaublad gebruiken voor toegang tot de rol en gedetailleerde foutinformatie extern weergeven. Gebruik de volgende stappen om de fouten met behulp van extern bureaublad weer te geven:

1. Zorg ervoor dat de Azure SDK 1.3 of hoger is geïnstalleerd.
2. Tijdens de implementatie van de oplossing met behulp van Visual Studio, ervoor kiezen om 'Configureren extern-bureaubladverbindingen'. Zie voor meer informatie over het configureren van de verbinding met extern bureaublad [met behulp van extern bureaublad met de Azure-rollen](../vs-azure-tools-remote-desktop-roles.md).
3. In de Microsoft Azure-portal, zodra het exemplaar de status van wordt **gereed**afstand verbinding met het exemplaar. Zie voor meer informatie over het gebruik van de extern bureaublad met Cloudservices [afstand verbinding met de rolinstanties](cloud-services-role-enable-remote-desktop-new-portal.md#remote-into-role-instances).
5. Meld u aan de virtuele machine met de referenties die zijn opgegeven tijdens de configuratie van extern bureaublad.
6. Open een opdrachtvenster.
7. Typ `IPconfig`.
8. Noteer de waarde van de IPV4-adres.
9. Open Internet Explorer.
10. Typ het adres en de naam van de webtoepassing. Bijvoorbeeld `http://<IPV4 Address>/default.aspx`.

Navigeren naar de website, wordt er nu meer uitgebreide foutberichten geretourneerd:

* Serverfout in '/' toepassing.
* Beschrijving: Er is een onverwerkte uitzondering opgetreden tijdens het uitvoeren van de huidige webaanvraag. Raadpleeg de stacktracering voor meer informatie over de fout en de oorsprong ervan in de code.
* Details van uitzondering: System.IO.FIleNotFoundException: kan niet worden geladen, bestand of assembly ' Microsoft.WindowsAzure.StorageClient, versie 1.1.0.0, Culture = neutral, PublicKeyToken = 31bf856ad364e35 =' of een van de afhankelijkheden ervan. Het systeem kan het opgegeven bestand niet vinden.

Bijvoorbeeld:

![Expliciete Serverfout in '/' toepassing](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503389.png)

## <a name="diagnose-issues-by-using-the-compute-emulator"></a>Problemen met behulp van de rekenemulator vaststellen
De Microsoft Azure-rekenemulator kunt u vaststellen en oplossen van problemen met ontbrekende afhankelijkheden en fouten van web.config.

Voor de beste resultaten bij het gebruik van deze methode van de diagnose van, moet u een computer of virtuele machine waarop een schone installatie van Windows is. Gebruik voor het beste de Azure-omgeving nabootsen, Windows Server 2008 R2 x64.

1. Installeer de zelfstandige versie van de [Azure SDK](https://azure.microsoft.com/downloads/).
2. Bouw het cloudserviceproject op de ontwikkelcomputer.
3. Navigeer in Windows Verkenner naar de map bin\debug van het cloudserviceproject.
4. Kopieer het .csx map- en .cscfg-bestand op de computer die u gebruikt voor fouten opsporen in de problemen.
5. Open een venster van de Azure SDK-opdrachtprompt en typ op de computer schoon `csrun.exe /devstore:start`.
6. Typ het volgende achter de opdrachtprompt `run csrun <path to .csx folder> <path to .cscfg file> /launchBrowser`.
7. Wanneer de functie wordt gestart, ziet u de gedetailleerde foutinformatie in Internet Explorer. U kunt ook de standaard-Windows-hulpprogramma's voor probleemoplossing gebruiken voor verdere diagnose van het probleem.

## <a name="diagnose-issues-by-using-intellitrace"></a>Problemen met behulp van IntelliTrace vaststellen
Voor worker en webservice-functies die er gebruik van .NET Framework 4, kunt u [IntelliTrace](https://msdn.microsoft.com/library/dd264915.aspx), die beschikbaar is in Microsoft Visual Studio Enterprise.

Volg deze stappen voor het implementeren van de service met IntelliTrace ingeschakeld:

1. Controleer of de Azure SDK 1.3 of hoger is geïnstalleerd.
2. De oplossing implementeren met behulp van Visual Studio. Controleer tijdens de implementatie van de **IntelliTrace inschakelen voor .NET 4 rollen** selectievakje.
3. Zodra het exemplaar wordt gestart, opent u de **Server Explorer**.
4. Vouw de **Azure\\Cloudservices** knooppunt en zoek de implementatie.
5. Vouw de implementatie uit totdat u de rolexemplaren ziet. Met de rechtermuisknop op een van de exemplaren.
6. Kies **weergave IntelliTrace-logboeken**. De **IntelliTrace samenvatting** wordt geopend.
7. Zoek de sectie uitzonderingen van de samenvatting. Als er uitzonderingen zijn, de sectie heet **uitzonderingsgegevens**.
8. Vouw de **uitzonderingsgegevens** en zoekt u **System.IO.FileNotFoundException** vergelijkbaar met de volgende fouten:

![Uitzonderingsgegevens, ontbreekt een bestand of assembly](./media/cloud-services-troubleshoot-roles-that-fail-start/ic503390.png)

## <a name="address-missing-dlls-and-assemblies"></a>Adres van de ontbrekende dll's en verzamelingen
Ontbrekende dll-bestand en assembly fouten oplossen, de volgende stappen uit:

1. Open de oplossing in Visual Studio.
2. In **Solution Explorer**, open de **verwijzingen** map.
3. Klik op de assembly die is geïdentificeerd in de fout.
4. In de **eigenschappen** deelvenster, zoek **lokale kopie van de eigenschap** en stel de waarde op **True**.
5. Implementeer de cloudservice opnieuw.

Zodra u hebt gecontroleerd dat alle fouten hebt gecorrigeerd, kunt u de service implementeren zonder te controleren of de **IntelliTrace inschakelen voor .NET 4 rollen** selectievakje.

## <a name="next-steps"></a>Volgende stappen
Meer [probleemoplossing artikelen](https://azure.microsoft.com/documentation/articles/?tag=top-support-issue&product=cloud-services) voor cloudservices.

Zie voor meer informatie over het oplossen van problemen met de rol van de cloud service met behulp van Azure PaaS computer diagnostics-gegevens, [blogreeks van Kevin Williamson](http://blogs.msdn.com/b/kwill/archive/2013/08/09/windows-azure-paas-compute-diagnostics-data.aspx).

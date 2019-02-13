---
title: 'Azure AD Connect: Wat is de ADConnectivityTool PowerShell-Module | Microsoft Docs'
description: Dit document beschrijft de nieuwe ADConnectivity PowerShell-module
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 10/19/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 52f8b70a4b098cc7266042499300235588e5303f
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56164308"
---
# <a name="what-is-the-adconnectivitytool-powershell-module"></a>Wat is de ADConnectivityTool PowerShell-Module?

Het hulpprogramma ADConnectivity is een PowerShell-module die wordt gebruikt in een van de volgende:

- Tijdens de installatie als een probleem met de netwerkverbinding wordt voorkomen dat een geslaagde validatie van de Active Directory-referenties voor de gebruiker is opgegeven in de Wizard.
- Na de installatie door een gebruiker die de functies vanuit een PowerShell-sessie aanroept.

Het hulpprogramma bevindt zich in: **C:\Program Files\Microsoft Azure Active Directory Connect\Tools\ ADConnectivityTool.psm1** 

## <a name="adconnectivitytool-during-installation"></a>ADConnectivityTool tijdens de installatie

Op de **verbinding maken met uw mappen** pagina in de Azure AD Connect-Wizard, als er een probleem optreedt, het ADConnectivityTool wordt automatisch gebruikt een van de functies om te bepalen wat er gebeurt.  Een van de volgende kan worden beschouwd als netwerkproblemen:

- De naam van het Forest van de gebruiker is opgegeven is verkeerd getypt of deze Forest bestaat niet 
- UDP-poort 389 is afgesloten in de domeincontrollers die zijn gekoppeld aan het Forest dat de gebruiker opgegeven
- De referenties die zijn opgegeven in het venster AD-forestaccount beschikt niet over machtigingen om op te halen van de domeincontrollers die zijn gekoppeld aan het doel Forest
- Een van de TCP-poort 53, 88 of 389 zijn in de domeincontrollers die zijn gekoppeld aan het Forest dat de gebruiker opgegeven gesloten 
- Zowel 389 UDP en TCP-poort (of -poorten) zijn gesloten
- DNS kan niet worden opgelost voor de opgegeven Forest en/of de bijbehorende domeincontrollers

Wanneer een van deze problemen worden gevonden, wordt een bijbehorende foutbericht wordt weergegeven in de AADConnect Wizard:


![Fout](media/how-to-connect-adconnectivitytools/error1.png)

Bijvoorbeeld weergegeven wanneer we bezig zijn met het toevoegen van een map op de **verbinding maken met uw mappen** scherm, Azure AD Connect nodig heeft om dit te controleren en om te communiceren met een domeincontroller via poort 389 verwacht.  Als dit niet het, zien we de fout die wordt weergegeven in de bovenstaande schermafbeelding.  

Wat er werkelijk gebeurt achter de schermen, is dat Azure AD Connect is aanroepen van de `Start-NetworkConnectivityDiagnosisTools` functie.  Deze functie wordt aangeroepen wanneer de validatie van referenties is mislukt vanwege een probleem met de netwerkverbinding.

Ten slotte een gedetailleerd logboekbestand gegenereerd wanneer het hulpprogramma wordt aangeroepen vanuit de wizard. Het logboek bevindt zich in **C:\ProgramData\AADConnect\ADConnectivityTool-<date>-<time>.log**

## <a name="adconnectivitytools-post-installation"></a>ADConnectivityTools na de installatie
Nadat Azure AD Connect is geïnstalleerd, kan een van de functies in de ADConnectivityTools PowerShell-module kan worden gebruikt.  

U vindt informatie over de functies in de [ADConnectivityTools verwijzing](reference-connect-adconnectivitytools.md)

### <a name="start-connectivityvalidation"></a>Start-ConnectivityValidation

We gaan deze functie aanroepen omdat het **alleen** handmatig worden aangeroepen zodra de ADConnectivityTool.psm1 is geïmporteerd in PowerShell. 

Deze functie wordt uitgevoerd dezelfde logica die de Azure AD Connect-Wizard uitvoert voor het valideren van de opgegeven AD-referenties.  Het biedt echter een nog veel meer uitgebreide uitleg over het probleem en een voorgestelde oplossing. 

De validatie van de verbinding bestaat uit de volgende stappen uit:
-   Domein-FQDN (volledig gekwalificeerde domeinnaam)-object ophalen
-   Valideren dat, als de gebruiker heeft geselecteerd maken nieuwe AD-account, deze referenties deel uitmaken van de groep Ondernemingsadministrators
-   Get-Forest FQDN-object
-   Bevestig dat ten minste één domein is gekoppeld aan de eerder verkregen Forest FQDN-object is bereikbaar
-   Controleer of het functionaliteitsniveau van het forest WindowsServer 2003 of hoger.

De gebruiker zich naar een map toe te voegen als al deze acties met succes zijn uitgevoerd.

Als de gebruiker deze functie wordt uitgevoerd nadat een probleem is opgelost (of als er geen probleem bestaat helemaal) in de uitvoer voor de gebruiker gaat u terug naar de Azure AD Connect-Wizard en probeer de referenties opnieuw in te voegen.



## <a name="next-steps"></a>Volgende stappen
- [Azure AD Connect: accounts en machtigingen](reference-connect-accounts-permissions.md)
- [Snelle installatie](how-to-connect-install-express.md)
- [Aangepaste installatie](how-to-connect-install-custom.md)
- [ADConnectivityTools verwijzing](reference-connect-adconnectivitytools.md)


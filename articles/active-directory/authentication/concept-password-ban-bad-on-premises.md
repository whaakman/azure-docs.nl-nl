---
title: Azure AD-wachtwoord protection preview
description: BEen zwakke wachtwoorden in de lokale Active Directory met behulp van de Azure AD wachtwoord protection preview
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: article
ms.date: 06/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 0819a947229e633331253923654de56638a6c76a
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36296111"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Voorbeeld: Azure AD-wachtwoordbeveiliging voor Windows Server Active Directory afdwingen

|     |
| --- |
| Azure AD-wachtwoordbeveiliging en de lijst met aangepaste verboden wachtwoorden zijn openbare preview-functies van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksvoorwaarden voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Azure AD-wachtwoordbeveiliging is een nieuwe functie in de openbare preview aangedreven door Azure Active Directory (Azure AD) voor het wachtwoordbeleid in een organisatie te verbeteren. De on-premises implementatie van Azure AD-wachtwoordbeveiliging maakt gebruik van zowel het algemene en aangepaste verboden lijsten met wachtwoorden opgeslagen in Azure AD en voert u de dezelfde controles on-premises als Azure AD cloud-gebaseerde wijzigingen.

Er zijn drie softwareonderdelen die gezamenlijk wachtwoordbeveiliging voor Azure AD:

* De Azure AD wachtwoord beveiliging proxy-service wordt uitgevoerd op een machine domein in de huidige Active Directory-forest. Het doorsturen van aanvragen van domeincontrollers naar Azure AD en het antwoord geretourneerd van Azure AD terug naar de domeincontroller.
* De agent-service van Azure AD wachtwoord beveiliging DC wachtwoord validatie verzoeken ontvangen van het filter-dll-bestand van DC-Agent wachtwoord, verwerkt met behulp van het huidige lokaal beschikbaar wachtwoordbeleid en retourneert het resultaat (pass\fail). Deze service is verantwoordelijk voor het periodiek (één keer per uur) aanroepen van de Azure AD wachtwoord beveiliging proxy-service om op te halen van nieuwe versies van het wachtwoordbeleid. -Communicatie voor aanroepen naar en van de Azure AD wachtwoord beveiliging proxy-service wordt uitgevoerd via RPC (Remote Procedure Call) via TCP. Bij het ophalen, wordt nieuwe beleid opgeslagen in een sysvol-map waar ze naar andere domeincontrollers repliceren kunnen. De DC-agent-service bewaakt ook de map sysvol op wijzigingen als andere domeincontrollers er nieuwe wachtwoordbeleid hebt geschreven, de controle van de proxy-service van Azure AD wachtwoord beveiliging wordt overgeslagen als een waarde recente beleid al beschikbaar is.
* Het filter-dll-bestand van DC-Agent wachtwoord wachtwoord validatie verzoeken ontvangen van het besturingssysteem en ze doorstuurt naar de Azure AD wachtwoord beveiliging DC agent-service die lokaal wordt uitgevoerd op de domeincontroller.

![Hoe Azure AD-wachtwoord beveiliging onderdelen samenwerken.](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

## <a name="requirements"></a>Vereisten

* Alle computers waarop Azure AD wachtwoord beveiliging onderdelen zijn geïnstalleerd met inbegrip van de domeincontrollers moeten worden uitgevoerd worden WindowsServer 2012 of later.
* -Netwerkverbinding bestaat tussen ten minste één domeincontroller in elk domein en ten minste één server die als host fungeert voor de proxy-service van Azure AD wachtwoord beveiliging.
* Alle Active Directory-domein met de domeincontroller service agentsoftware DFSR moet gebruiken voor sysvol-replicatie.
* Een globale beheerdersaccount om u te registreren van de proxy-service van Azure AD wachtwoord beveiliging met Azure AD.
* Een account met domeinadministratorrechten Active Directory in het foresthoofddomein.

### <a name="license-requirements"></a>Licentievereisten

De voordelen van de lijst met algemene verboden wachtwoorden gelden voor alle gebruikers van Azure Active Directory (Azure AD).

De lijst met aangepaste verboden wachtwoorden vereist Azure AD Basic licenties.

Azure AD-wachtwoordbeveiliging voor Windows Server Active Directory vereist Azure AD Premium-licenties. 

Aanvullende licentie-informatie, inclusief kosten, kunt u vinden op de [Azure Active Directory website prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Downloaden

Er zijn twee vereiste installatieprogramma's voor Azure AD-wachtwoordbeveiliging die kan worden gedownload vanaf de [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=57071)

## <a name="answers-to-common-questions"></a>Antwoorden op veelgestelde vragen

* Er is geen verbinding met internet vereist naar de domeincontrollers. De computer (s) met de proxy-service van Azure AD wachtwoord beveiliging zijn de enige machines dat internetconnectiviteit vereist.
* Er is geen netwerkpoorten worden geopend op domeincontrollers.
* Er zijn geen wijzigingen in Active Directory-schema zijn vereist.
   * De software gebruikt de bestaande Active Directory-container en serviceConnectionPoint schema-objecten.
* Er is geen Active Directory-domein of Forest functionele niveau (DFL\FFL) minimumvereisten.
* De software niet maken of accounts in Active Directory-domeinen die het beveiligt vereisen.
* Stapsgewijze implementatie wordt ondersteund daar staat tegenover dat wachtwoordbeleid alleen wordt afgedwongen waar de domain controller-agent is geïnstalleerd.
* Azure AD-wachtwoordbeveiliging is niet een real-time beleidsengine van toepassing. Mogelijk zijn er een vertraging in de tijd tussen een wachtwoordwijziging voor de configuratie van beleid en de keer is bereikt en wordt afgedwongen op alle domeincontrollers.

## <a name="next-steps"></a>Volgende stappen

[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises.md)
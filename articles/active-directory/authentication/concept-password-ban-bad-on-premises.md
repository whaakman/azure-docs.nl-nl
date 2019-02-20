---
title: Azure AD wachtwoord protection preview
description: Verbieden zwakke wachtwoorden in on-premises Active Directory met behulp van de Azure AD wachtwoord protection preview
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/18/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1beae186f6eb276b9aa302d3d51f0ba8688e591
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415745"
---
# <a name="preview-enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Preview: Afdwingen van de beveiliging van Azure AD-wachtwoord voor Windows Server Active Directory

|     |
| --- |
| Beveiliging van Azure AD-wachtwoord en de lijst met aangepaste uitgesloten wachtwoorden zijn openbare preview-functies van Azure Active Directory. Zie voor meer informatie over previews [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-Previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Beveiliging van Azure AD-wachtwoord is een nieuwe functie in openbare preview, mogelijk gemaakt door Azure Active Directory (Azure AD) voor het verbeteren van wachtwoordbeleid in een organisatie. De on-premises implementatie van Azure AD-wachtwoordbeveiliging maakt gebruik van zowel het algemene en aangepaste lijsten met wachtwoorden die zijn opgeslagen in Azure AD geblokkeerd en de dezelfde controles on-premises als Azure AD cloud-gebaseerde wijzigingen uitvoert.

## <a name="design-principles"></a>Ontwerpprincipes

Azure AD-wachtwoord beveiliging voor Active Directory is ontworpen met de volgende principes in gedachten:

* Domeincontrollers zijn niet vereist om te communiceren rechtstreeks met Internet
* Er zijn geen nieuwe netwerkpoorten worden geopend op domeincontrollers.
* Er zijn geen wijzigingen in Active Directory-schema zijn vereist. De software maakt gebruik van de bestaande Active Directory-container en serviceConnectionPoint schemaobjecten.
* Er is geen minimale Active Directory-domein of Forest functionele niveau (DFL\FFL) is vereist.
* De software niet maken of vereisen dat alle accounts in Active Directory-domeinen die worden beschermd.
* Wachtwoorden van gebruikers niet-versleutelde tekst verlaten nooit de domeincontroller (of tijdens het wachtwoord validatie bewerkingen of op enig ander moment).
* Stapsgewijze implementatie wordt ondersteund daar staat tegenover dat beleid van het wachtwoord alleen wordt afgedwongen waar de domain controller-agent is geïnstalleerd.
* Het verdient aanbeveling de DC-agent installeren op alle DC's om ervoor te zorgen alomtegenwoordige wachtwoord beveiliging afdwingen.

## <a name="architectural-diagram"></a>Architectuurdiagram

Het is belangrijk dat u hebt een goed begrip van de onderliggende ontwerp- en functionele concepten voordat u Azure AD-wachtwoord beveiliging in een on-premises Active Directory-omgeving implementeert. Het volgende diagram toont hoe de onderdelen van Azure AD-wachtwoord Protection samenwerken:

![Hoe Azure AD wachtwoord beveiliging onderdelen samenwerken.](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

Het bovenstaande diagram toont de drie eenvoudige softwareonderdelen die gezamenlijk wachtwoordbeveiliging voor Azure AD:

* De Azure AD-wachtwoord beveiliging Proxy-service wordt uitgevoerd op elke machine domein in de huidige Active Directory-forest. Het primaire doel is het doorsturen van aanvragen van wachtwoord voor downloaden van domeincontrollers naar Azure AD en het antwoord geretourneerd uit Azure AD terug naar de domeincontroller.
* De Azure AD-wachtwoord DC Protection Agent wachtwoord filter-dll gebruikersaanvragen wachtwoord validatie van het besturingssysteem ontvangt en ze doorstuurt naar de Azure AD-wachtwoord beveiliging DC-Agent-service die lokaal wordt uitgevoerd op de domeincontroller.
* De Azure AD-wachtwoord DC Protection Agent-service ontvangt verzoeken van wachtwoord-validatie van het DLL-bestand van DC-Agent wachtwoord filter, verwerkt met behulp van de huidige (lokaal beschikbaar) wachtwoordbeleid en retourneert het resultaat (pass\fail).

## <a name="theory-of-operations"></a>Theorie van bewerkingen

Dus het bovenstaande diagram- en ontwerpprincipes gegeven hoe Azure AD-wachtwoord beveiliging daadwerkelijk werkt?

Elke Azure AD-wachtwoord beveiliging Proxy-service bekendmaakt op domeincontrollers in het forest met een object serviceConnectionPoint maken in Active Directory.

Elke Azure AD-wachtwoord DC Protection Agent-service maakt ook een object serviceConnectionPoint in Active Directory. Maar dit wordt primair gebruikt voor rapportage- en diagnostische gegevens.

De Azure AD-wachtwoord DC Protection Agent-service is verantwoordelijk voor het initiëren van het downloaden van een nieuw wachtwoordbeleid van Azure AD. De eerste stap is het lokaliseren van een Azure AD-wachtwoord beveiliging Proxy-service door het opvragen van het forest voor proxy serviceConnectionPoint objecten. Als een beschikbare proxyservice wordt gevonden, wordt een aanvraag voor het wachtwoord beleid downloaden van de DC-agent-service verzonden naar de proxyservice, die op zijn beurt verzendt die gegevens naar Azure AD en retourneert vervolgens het antwoord op de DC-agent-service. Na de ontvangst van een nieuw wachtwoordbeleid van Azure AD, slaat de DC-agent-service het beleid in een specifieke map in de hoofdmap van de sysvol-share van het domein. De DC-agent-service bewaakt ook deze map in het geval nieuwere beleid van andere services van de agent DC in het domein repliceren.

De Azure AD-wachtwoord DC Protection Agent-service vraagt altijd om een nieuw beleid op de service is gestart. Nadat de DC-Agent-service is gestart, wordt er periodiek (één keer per uur) Controleer de leeftijd van de huidige lokaal beschikbaar beleid; Als het huidige beleid ouder is dan een uur de DC-agent-service wordt aanvragen een nieuw beleid uit Azure AD, zoals hierboven beschreven, anders wordt de DC-agent wordt echter ook doorgaan met het huidige beleid.

De service Azure AD-wachtwoord-DC beveiligingsagent communiceert met de Azure AD-wachtwoord beveiliging Proxy-service met behulp van RPC (Remote Procedure Call) via TCP. De Proxy-service luistert naar deze aanroepen op een van beide een dynamisch of statisch RPC-poort (zoals geconfigureerd).

De Azure AD-wachtwoord beveiliging DC-Agent niet op een netwerk beschikbaar poort luistert en de Proxy-service probeert nooit aan te roepen de DC-agent-service.

De Azure AD-wachtwoord beveiliging Proxy-service is staatloos; deze nooit in de cache opgeslagen beleid of een andere staat gedownload van Azure.

De Azure AD-wachtwoord DC Protection Agent-service evalueert alleen ooit het wachtwoord van een gebruiker met behulp van de meest recente lokaal beschikbaar wachtwoordbeleid. Als er geen wachtwoordbeleid beschikbaar op de lokale domeincontroller is, het wachtwoord wordt automatisch kan worden geaccepteerd en een bericht van gebeurtenislogboek worden geregistreerd om te waarschuwen de beheerder.

Beveiliging van Azure AD-wachtwoord is niet een realtime toepassing beleidsengine. Er is mogelijk een vertraging in de tijd tussen een wachtwoordwijziging voor de configuratie van beleid wordt gemaakt in Azure AD en de tijd die het heeft bereikt en op alle domeincontrollers wordt afgedwongen.

## <a name="foresttenant-binding-for-azure-ad-password-protection"></a>Forest\tenant binding voor de beveiliging van Azure AD-wachtwoord

Implementatie van de beveiliging van Azure AD-wachtwoord in een Active Directory-forest moet de registratie van het Active Directory-forest en alle geïmplementeerde Azure AD-wachtwoord beveiliging proxyservices, met Azure AD. Beide dergelijke registraties (forest en proxy's) zijn gekoppeld aan een specifieke Azure AD-tenant die impliciet via de referenties die worden gebruikt tijdens de registratie wordt geïdentificeerd. Telkens wanneer een wachtwoord van Azure AD-wachtwoord beveiligingsbeleid is gedownload, is het altijd specifiek voor deze tenant (dat wil zeggen, dat beleid wordt altijd een combinatie van de globale Microsoft uitgesloten wachtwoord en de lijst van de aangepaste uitgesloten wachtwoorden per tenant). Het wordt niet ondersteund voor het configureren van verschillende domeinen of proxy's in een forest worden gebonden aan andere Azure AD tenants.

## <a name="license-requirements"></a>Licentievereisten

De voordelen van de lijst met uitgesloten wachtwoorden globale gelden voor alle gebruikers van Azure Active Directory (Azure AD).

De lijst met aangepaste uitgesloten wachtwoorden vereist Azure AD Basic-licenties.

Azure AD-wachtwoord beveiliging voor Windows Server Active Directory vereist Azure AD Premium-licenties.

Extra licentie-informatie, inclusief kosten, vindt u op de [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Downloaden

De twee vereist agent-installatieprogramma's voor de Azure AD-wachtwoord beveiliging die kan worden gedownload vanaf de [Microsoft Downloadcentrum](https://www.microsoft.com/download/details.aspx?id=57071)

## <a name="next-steps"></a>Volgende stappen

[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises-deploy.md)

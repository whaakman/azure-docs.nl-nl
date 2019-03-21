---
title: Wachtwoordbeveiliging in Azure AD
description: Zwakke wachtwoorden in on-premises Active Directory met behulp van Azure AD wachtwoordbeveiliging blokkeren
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
ms.openlocfilehash: f1b3660d256e4beda948f723035aa75ca8a9ed2e
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58284865"
---
# <a name="enforce-azure-ad-password-protection-for-windows-server-active-directory"></a>Afdwingen van de beveiliging van Azure AD-wachtwoord voor Windows Server Active Directory

Beveiliging van Azure AD-wachtwoord is een functie waarmee wachtwoordbeleid in een organisatie. On-premises implementatie wachtwoordbeveiliging maakt gebruik van de twee algemene en aangepaste verboden wachtwoord lijsten die zijn opgeslagen in Azure AD. Hiervoor wordt de dezelfde controles on-premises als Azure AD voor cloud-gebaseerde wijzigingen.

## <a name="design-principles"></a>Ontwerpprincipes

Azure AD-wachtwoordbeveiliging wordt gekenmerkt door deze principes in gedachten:

* Domeincontrollers wordt nooit meer hoeft te communiceren rechtstreeks met internet.
* Er zijn geen nieuwe netwerkpoorten worden geopend op domeincontrollers.
* Er zijn geen wijzigingen in Active Directory-schema zijn vereist. De software maakt gebruik van de bestaande Active Directory **container** en **serviceConnectionPoint** schemaobjecten.
* Er is geen minimale Active Directory-domein of forest functionele niveau (DFL/FFL) is vereist.
* De software niet maken of vereisen dat accounts in Active Directory-domeinen die ermee worden beveiligd.
* Wachtwoorden van gebruikers niet-versleutelde tekst laat niet u de domeincontroller tijdens wachtwoord validatie bewerkingen of op enig ander moment.
* Stapsgewijze implementatie wordt ondersteund. Maar het wachtwoordbeleid is alleen afgedwongen waarop de Agent in Domain Controller (DC-Agent) is geïnstalleerd.
* Het is raadzaam dat u de DC-Agent installeren op alle domeincontrollers om ervoor te zorgen universal wachtwoord beveiliging afdwingen.

## <a name="architectural-diagram"></a>Architectuurdiagram

Het is belangrijk te begrijpen van de onderliggende ontwerp en de functieconcepten voordat u de beveiliging van Azure AD-wachtwoorden in een on-premises Active Directory-omgeving implementeert. Het volgende diagram toont hoe de onderdelen van wachtwoordbeveiliging samenwerken:

![Hoe Azure AD wachtwoord beveiliging onderdelen samenwerken.](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

* De Azure AD-wachtwoord beveiliging Proxy-service wordt uitgevoerd op elke machine domein in de huidige Active Directory-forest. Het primaire doel hiervan is voor het doorsturen van aanvragen van wachtwoord voor downloaden van domeincontrollers naar Azure AD. Deze retourneert vervolgens de antwoorden van Azure AD naar de domeincontroller.
* De wachtwoordfilter-DLL van de DC-Agent ontvangt gebruikersaanvragen voor validatie van het wachtwoord van het besturingssysteem. Deze stuurt ze door naar de DC-Agent-service die lokaal wordt uitgevoerd op de domeincontroller.
* De DC-Agent-service van wachtwoordbeveiliging ontvangt verzoeken van de validatie van het wachtwoord van de wachtwoordfilter-DLL van de DC-Agent. Ze worden verwerkt met behulp van de huidige (lokaal beschikbaar) wachtwoordbeleid en retourneert het resultaat: *doorgeven* of *mislukken*.

## <a name="how-password-protection-works"></a>De werking van wachtwoordbeveiliging

Elk service-exemplaar van Azure AD-wachtwoord beveiliging Proxy kondigt zichzelf naar de domeincontrollers in het forest met het maken van een **serviceConnectionPoint** -object in Active Directory.

Elke DC-agentservice voor wachtwoordbeveiliging maakt ook een **serviceConnectionPoint** -object in Active Directory. Dit object wordt voornamelijk voor rapportage- en diagnostische gegevens gebruikt.

De DC-Agent-service is verantwoordelijk voor het initiëren van het downloaden van een nieuw wachtwoordbeleid van Azure AD. De eerste stap is het lokaliseren van een Azure AD-wachtwoord beveiliging Proxy-service door het opvragen van het forest voor proxy **serviceConnectionPoint** objecten. Als een beschikbare proxyservice wordt gevonden, verzendt de DC-Agent downloaden beleidsverzoek van een wachtwoord naar de proxy-service. De proxy-service wordt op zijn beurt de aanvraag verzendt naar Azure AD. De proxy-service retourneert vervolgens het antwoord op de DC-Agent-service.

Nadat de DC-Agent-service een nieuw wachtwoordbeleid van Azure AD ontvangt, de service wordt het beleid opgeslagen in een specifieke map in de hoofdmap van het domein *sysvol* map delen. De DC-Agent-service bewaakt ook deze map in het geval nieuwere beleid van andere services DC-Agent in het domein repliceren.

De DC-Agent-service aanvragen altijd een nieuw beleid op de service is gestart. Nadat de DC-Agent-service is gestart, wordt de leeftijd van de huidige lokaal beschikbaar beleid per uur gecontroleerd. Als het beleid ouder dan een uur is, vraagt de DC-Agent een nieuw beleid uit Azure AD, zoals eerder beschreven. Als het huidige beleid niet ouder is dan een uur is, blijft de DC-Agent gebruiken dat beleid.

Wanneer een Azure AD-hulpprogramma voor het wachtwoord van de wachtwoord-beveiligingsbeleid is gedownload, wordt dat beleid is uitsluitend bestemd voor een tenant. Met andere woorden, zijn wachtwoordbeleid altijd een combinatie van de lijst met Microsoft global verboden wachtwoord en de lijst met per tenant aangepaste verboden-wachtwoord.

De DC-Agent communiceert met de proxy-service via RPC via TCP. De proxy-service luistert naar deze aanroepen op een dynamisch of statisch RPC-poort, afhankelijk van de configuratie.

De DC-Agent is nooit op een netwerk beschikbaar poort luistert.

De proxy-service aanroepen nooit de DC-Agent-service.

De proxy-service is staatloos. Deze nooit in de cache opgeslagen beleid of een andere staat gedownload van Azure.

De DC-Agent-service gebruikt altijd de meest recente lokaal beschikbaar wachtwoordbeleid om te evalueren van het wachtwoord van een gebruiker. Als geen wachtwoordbeleid op de lokale domeincontroller beschikbaar is, wordt het wachtwoord automatisch geaccepteerd. Wanneer dit gebeurt, wordt een bericht wordt geregistreerd om te waarschuwen de beheerder.

Beveiliging van Azure AD-wachtwoord is niet een realtime toepassing beleidsengine. Er is een vertraging tussen wanneer een wachtwoordwijziging voor de configuratie van beleid wordt gemaakt in Azure AD en wanneer bereikt wijzigen en op alle domeincontrollers wordt afgedwongen.

## <a name="foresttenant-binding-for-password-protection"></a>Forest/tenant-binding voor beveiliging met een wachtwoord

Implementatie van de beveiliging van Azure AD-wachtwoord in een Active Directory-forest moet de registratie van dit forest met Azure AD. Elke proxyservice die wordt gedistribueerd moet ook worden geregistreerd bij Azure AD. Deze forest- en proxy-registraties zijn gekoppeld aan een specifieke Azure AD-tenant, die impliciet wordt geïdentificeerd door de referenties die worden gebruikt tijdens de registratie.

Het Active Directory-forest en alle geïmplementeerde proxyservices binnen een forest moeten worden geregistreerd bij dezelfde tenant. Een Active Directory-forest of proxyservices hebben in dat forest wordt geregistreerd bij andere Azure AD-tenants wordt niet ondersteund. Symptomen van een verkeerd geconfigureerde implementatie zijn de modeldatabase groter voor het downloaden van wachtwoordbeleid.

## <a name="license-requirements"></a>Licentievereisten

De voordelen van de lijst met uitgesloten wachtwoorden globale gelden voor alle gebruikers van Azure AD.

De aangepaste lijst met uitgesloten-wachtwoorden vereist Azure AD Basic-licenties.

Azure AD-wachtwoordbeveiliging voor Windows Server Active Directory vereist Azure AD Premium-licenties.

Voor extra licentie-informatie, Zie [prijzen van Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="download"></a>Downloaden

De twee vereiste agent-installatieprogramma's voor beveiliging van Azure AD-wachtwoorden zijn beschikbaar via de [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071).

## <a name="next-steps"></a>Volgende stappen
[Wachtwoordbeveiliging in Azure AD implementeren](howto-password-ban-bad-on-premises-deploy.md)

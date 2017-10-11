---
title: LDAP-verificatie en Azure MFA Server | Microsoft Docs
description: Dit is de pagina Azure Multi-Factor Authentication die u helpt bij het implementeren van LDAP-verificatie en de Azure Multi-Factor Authentication-server.
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: e1a68568-53d1-4365-9e41-50925ad00869
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: kgremban
ms.openlocfilehash: 8f4d5f9e84ad7bb4fff501370036e7f0da589bf3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="ldap-authentication-and-azure-multi-factor-authentication-server"></a>LDAP-verificatie en Azure multi-factor Authentication-Server
Standaard is de Azure Multi-Factor Authentication-server geconfigureerd om de gebruikers van Active Directory te importeren of te synchroniseren. Deze kan echter worden geconfigureerd om verbinding te maken met verschillende LDAP-adreslijsten, zoals een ADAM-adreslijst of een specifieke Active Directory-domeincontroller. Wanneer verbonden met een directory via LDAP, wordt de Azure multi-factor Authentication-Server kan fungeren als een LDAP-proxy om uit te voeren verificaties. Daarnaast kunt u de LDAP-binding gebruiken als een RADIUS-doel voor het uitvoeren van pre-verificaties van gebruikers met IIS-verificatie, of voor het uitvoeren van primaire verificaties in de Azure MFA-gebruikersportal.

Plaats voor het gebruik van Azure multi-factor Authentication als een LDAP-proxy, de Azure multi-factor Authentication-Server tussen de LDAP-client (bijvoorbeeld VPN-apparaat, toepassing) en de LDAP-adreslijstserver. De Azure Multi-Factor Authentication-server moet worden geconfigureerd om te kunnen communiceren met zowel de clientservers als de LDAP-directory. In deze configuratie accepteert de Azure Multi-Factor Authentication-server LDAP-aanvragen van clientservers en toepassingen, en stuurt deze door naar de doel-LDAP-adreslijstserver om de primaire referenties te valideren. Als de LDAP-adreslijst wordt de primaire referenties gevalideerd, wordt Azure multi-factor Authentication wordt een tweede ID-verificatie wordt uitgevoerd en stuurt een antwoord terug naar de LDAP-client. De volledige verificatie slaagt alleen als de LDAP-serververificatie en de verificatie in de tweede stap zijn gelukt.

## <a name="configure-ldap-authentication"></a>LDAP-verificatie configureren
Als u LDAP-verificatie wilt configureren, moet u de Azure Multi-Factor Authentication-server op een Windows-server installeren. Gebruik de volgende procedure:

### <a name="add-an-ldap-client"></a>Een LDAP-client toevoegen

1. Selecteer het pictogram LDAP-verificatie in het menu links in de Azure multi-factor Authentication-Server.
2. Schakel het selectievakje **LDAP-verificatie** in.

   ![LDAP-verificatie](./media/multi-factor-authentication-get-started-server-ldap/ldap2.png)

3. Wijzig op het tabblad Clients de TCP-poort en de SSL-poort als de LDAP-service van Azure Multi-Factor Authentication verbinding moet maken met niet-standaardpoorten om te luisteren naar LDAP-aanvragen.
4. Als u van plan bent LDAPS te gebruiken vanaf de client naar de Azure multi-factor Authentication-Server, moet u een SSL-certificaat geïnstalleerd op dezelfde server als de MFA-Server. Klik op **Bladeren** naast de SSL-certificaat vak en selecteer een certificaat wilt gebruiken voor de beveiligde verbinding.
5. Klik op **Add**.
6. Voer in het dialoogvenster LDAP-Client toevoegen het IP-adres van het toestel, server of toepassing die wordt geverifieerd met de Server en een toepassingsnaam in (optioneel). De naam van de toepassing wordt vermeld in Azure Multi-Factor Authentication-rapporten en kan worden weergegeven in verificatieberichten via sms of mobiele apps.
7. Schakel het selectievakje **Overeenkomende Azure Multi-Factor Authentication-gebruiker** vereisen in als alle gebruikers zijn of moeten worden geïmporteerd in de server en aan verificatie in twee stappen onderworpen zijn. Als een groot aantal gebruikers nog niet is geïmporteerd in de Server en/of vrijgesteld van verificatie in twee stappen zijn, laat u het vakje uitgeschakeld. Zie de help-bestand voor meer informatie over deze functie MFA-Server.

Herhaal deze stappen om extra LDAP-clients toe te voegen.

### <a name="configure-the-ldap-directory-connection"></a>De LDAP-directoryverbinding configureren

Wanneer Azure Multi-Factor Authentication is geconfigureerd om LDAP-verificaties te kunnen ontvangen, moet deze als proxy optreden voor verificaties bij de LDAP-adreslijst. Dat is de reden waarom op het tabblad Doel een enkele niet beschikbare optie voor het gebruik van een LDAP-doel wordt weergegeven.

1. Als u de verbinding voor de LDAP-directory wilt configureren, klikt u op het pictogram **Directoryintegratie**.
2. Schakel op het tabblad Instellingen het keuzerondje **Specifieke LDAP-configuratie** gebruiken in.
3. Selecteer **Bewerken...**
4. Vul in de velden van het dialoogvenster LDAP-configuratie bewerken de benodigde informatie in om verbinding te maken met de LDAP-adreslijst. Beschrijvingen van de velden zijn ook beschikbaar in het Help-bestand voor de Azure Multi-Factor Authentication-server.

    ![Adreslijstintegratie](./media/multi-factor-authentication-get-started-server-ldap/ldap.png)

5. Test de LDAP-verbinding door te klikken op de knop **Testen**.
6. Als de LDAP-verbindingstest is geslaagd, klikt u op de knop **OK**.
7. Klik op het tabblad **Filters**. De server is vooraf geconfigureerd om containers, beveiligingsgroepen en gebruikers van Active Directory te laden. Als u verbinding maakt met een andere LDAP-directory, moet u waarschijnlijk de weergegeven filters bewerken. Klik op de koppeling **Help** voor meer informatie over filters.
8. Klik op het tabblad **Kenmerken**. De Azure Multi-Factor Authentication-server is vooraf geconfigureerd om kenmerken van Active Directory toe te wijzen.
9. Als u verbinding maakt met een andere LDAP-directory of als u de vooraf geconfigureerde kenmerktoewijzingen wilt wijzigen, klikt u op **Bewerken...**
10. Wijzig in het dialoogvenster Kenmerken bewerken de toewijzingen van de LDAP-kenmerktoewijzingen voor uw adreslijst. Kenmerknamen kunnen worden getypt of geselecteerd door te klikken op de **...** -knop naast elk veld. Klik op de koppeling **Help** voor meer informatie over kenmerken.
11. Klik op de knop **OK**.
12. Klik op het pictogram **Bedrijfsinstellingen** en selecteer het tabblad **Gebruikersnaamomzetting**.
13. Als u een verbinding maakt met Active Directory vanaf een server die is gekoppeld aan een domein, laat u het keuzerondje **Windows-beveiligings-id's (SID's) gebruiken voor overeenkomende gebruikersnamen** ingeschakeld. Schakel anders het keuzerondje **Het kenmerk Unieke LDAP-id gebruiken voor overeenkomende gebruikersnamen** in. 

Wanneer het keuzerondje **Het kenmerk Unieke LDAP-id gebruiken voor overeenkomende gebruikersnamen** is geselecteerd, probeert de Azure Multi-Factor Authentication-server elke gebruikersnaam om te zetten in een unieke id in de LDAP-directory. Een LDAP-zoekopdracht wordt uitgevoerd op de gebruikersnaamkenmerken die zijn gedefinieerd op het tabblad Directoryintegratie > Kenmerken. Wanneer een gebruiker zich verifieert, wordt de gebruikersnaam omgezet naar de unieke id in de LDAP-adreslijst. De unieke id wordt gebruikt voor het afstemmen van de gebruiker in de Azure multi-factor Authentication-gegevensbestand. Hierdoor kunnen niet-hoofdlettergevoelige vergelijkingen en lange en korte gebruikersnaamnotaties worden gebruikt.

Nadat u deze stappen hebt voltooid, wordt de MFA-Server luistert op de geconfigureerde poorten naar toegangsaanvragen van LDAP van de geconfigureerde clients, en fungeert als proxy voor deze aanvragen voor de LDAP-adreslijst voor verificatie.

## <a name="configure-ldap-client"></a>LDAP-client configureren
Als u de LDAP-client wilt configureren, gebruikt u de volgende richtlijnen:

* Configureer uw apparaat, server of toepassing als u wilt dat deze via LDAP bij de Azure Multi-Factor Authentication-server wordt geverifieerd alsof het uw LDAP was. Gebruik dezelfde instellingen die u normaal gesproken gebruikt om rechtstreeks verbinding te maken met de LDAP-directory, met uitzondering van de servernaam of het IP-adres omdat deze toebehoren aan de Azure Multi-Factor Authentication-server.
* De time-out voor LDAP in op 30 tot 60 seconden zodanig configureren dat er is nu tijd om de referenties van de gebruiker met de LDAP-adreslijst te valideren, uitvoeren van de tweede stap-verificatie, hun reactie ontvangen en reageren op de LDAP-toegangsaanvraag.
* Als LDAPS wordt gebruikt, moet het apparaat of de server die de LDAP-query's maakt het SSL-certificaat kunnen vertrouwen dat op de Azure Multi-Factor Authentication-server is geïnstalleerd.


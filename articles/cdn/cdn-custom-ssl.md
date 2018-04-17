---
title: HTTPS configureren op een aangepaste Azure CDN-domein | Microsoft Docs
description: Informatie over het in- of uitschakelen van HTTPS op uw aangepaste domein van Azure CDN-eindpunt.
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: rli; v-deasim
ms.openlocfilehash: ca3dad18973197f63e69e6568b8ea5988b279e01
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="configure-https-on-an-azure-cdn-custom-domain"></a>HTTPS configureren op een aangepaste Azure CDN-domein

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Azure Content Delivery Network (CDN) ondersteunt het HTTPS-protocol voor een aangepast domein op een CDN-eindpunt. Met het HTTPS-protocol op uw aangepaste domein, zorgt u ervoor dat uw vertrouwelijke gegevens wordt geleverd veilig via SSL-codering wanneer ze via internet worden verzonden. HTTPS biedt vertrouwt, verificatie, en uw webtoepassingen worden beschermd tegen aanvallen. Bovendien het leveren van de beveiligde inhoud met behulp van uw eigen domeinnaam (bijvoorbeeld https:\//www.contoso.com). De werkstroom inschakelen HTTPS wordt vereenvoudigd, via één klik inschakelen en volledige Certificaatbeheer, allemaal zonder extra kosten.

Azure CDN ondersteunt ook HTTPS op de hostnaam van een CDN-eindpunt standaard. Bijvoorbeeld, als u een CDN-eindpunt maken (zoals https:\//contoso.azureedge.net), HTTPS is automatisch ingeschakeld.  

Enkele van de belangrijkste kenmerken van de functie voor HTTPS zijn:

- Kan zonder extra kosten: Er zijn geen kosten voor het verkrijgen van het certificaat of vernieuwen en zonder extra kosten voor HTTPS-verkeer. U betaalt alleen voor GB uitgaande vanaf de CDN.

- Eenvoudige activering: één muisklik inrichting is beschikbaar via de [Azure-portal](https://portal.azure.com). U kunt ook REST-API of andere ontwikkelhulpprogramma's gebruiken de functie in te schakelen.

- Voltooien van Certificaatbeheer: alle aanschaf van het certificaat en u wordt beheerd. Certificaten worden automatisch worden ingericht en verlengd vóór de vervaldatum, waardoor de risico's van de service wordt onderbroken vanwege een certificaat verloopt.

>[!NOTE] 
>Voordat het HTTPS-ondersteuning wordt ingeschakeld, u moet hebben al een [aangepaste Azure CDN-domein](./cdn-map-content-to-custom-domain.md).

## <a name="enabling-https"></a>HTTPS inschakelen

Volg deze stappen zodat HTTPS op een aangepast domein:

### <a name="step-1-enable-the-feature"></a>Stap 1: De functie inschakelen 

1. In de [Azure-portal](https://portal.azure.com), blader naar uw **Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon** CDN-profiel.

2. Klik op het eindpunt met uw aangepaste domein in de lijst met eindpunten.

3. Klik op het aangepaste domein waarvoor u wilt inschakelen van HTTPS.

    ![Lijst met aangepaste domeinen](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. Klik op **op** om HTTPS, klikt u vervolgens op **toepassen**.

    ![Aangepast domein HTTPS-status](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


### <a name="step-2-validate-domain"></a>Stap 2: Domein valideren

>[!NOTE]
>Als u een certificaat autoriteit autorisatie (CAA)-record met uw DNS-provider hebt, moet het DigiCert bevatten als een geldige CA. Een record CAA kan eigenaren van een domein met hun DNS-providers die CA's zijn gemachtigd voor het verlenen van certificaten voor hun domein opgeven. Als een CA ontvangt een volgorde voor een certificaat voor een domein met een record CAA en die CA niet wordt vermeld als een gemachtigde uitgever, is het verboden van het certificaat heeft uitgegeven aan het domein of subdomein. Zie voor meer informatie over het beheren van CAA records [beheren CAA records](https://support.dnsimple.com/articles/manage-caa-record/). Zie voor een hulpprogramma van de record CAA [CAA Record Helper](https://sslmate.com/caa/).

#### <a name="custom-domain-is-mapped-to-cdn-endpoint"></a>Het aangepaste domein is toegewezen aan de CDN-eindpunt

Wanneer u een aangepast domein toegevoegd aan uw eindpunt, moet u een CNAME-record in de DNS-tabel van uw domeinregistrar om toe te wijzen naar de hostnaam van uw CDN-eindpunt gemaakt. Als dat CNAME-record bestaat nog steeds en niet het subdomein cdnverify bevat, de DigiCert-certificeringsinstantie (CA) wordt gebruikt voor het valideren eigenaar van uw aangepaste domein. 

De CNAME-record moet in de volgende indeling hebben: waar *naam* is uw aangepaste domeinnaam en *waarde* is de hostnaam van uw CDN-eindpunt:

| Naam            | Type  | Waarde                 |
|-----------------|-------|-----------------------|
| www.contoso.com | CNAME | contoso.azureedge.net |

Zie voor meer informatie over de CNAME-records [maakt u de DNS CNAME-record](https://docs.microsoft.com/en-us/azure/cdn/cdn-map-content-to-custom-domain#step-2-create-the-cname-dns-records).

Als uw CNAME-record in de juiste indeling is, wordt DigiCert automatisch gecontroleerd van uw aangepaste domeinnaam en voegt het toe aan het certificaat onderwerp alternatieve namen (SAN). DigitCert won't sturen je een bevestigingsmail en hoeft u uw aanvraag goedkeuren. Het certificaat is één jaar geldig en wordt automatische-vernieuwd voordat deze verloopt. Ga door naar [stap 3: wachten op doorgeven](#step-3-wait-for-propagation). 

De validatie van de automatische duurt normaal gesproken een paar minuten. Als u uw domein gevalideerd binnen een uur niet ziet, moet u een ondersteuningsticket opent.

#### <a name="cname-record-is-not-mapped-to-cdn-endpoint"></a>CNAME-record is niet toegewezen aan de CDN-eindpunt

Als de vermelding CNAME-record voor uw eindpunt niet langer bestaat of het subdomein cdnverify bevat, voert u de rest van de instructies in deze stap.

Nadat u HTTPS op uw aangepaste domein inschakelt, de DigiCert-certificeringsinstantie (CA) eigendom van uw domein contact opnemen met de bijbehorende registrant valideert volgens het domein [WHOIS](http://whois.domaintools.com/) registrant informatie. Neem contact op met worden uitgevoerd via het e-mailadres (standaard) of het telefoonnummer dat wordt vermeld in de WHOIS-registratie. U moet domeinvalidatie voltooien voordat HTTPS actief op uw aangepaste domein zijn. U hebt zes werkdagen goedkeuren van het domein. Aanvragen die niet binnen zes werkdagen zijn goedgekeurd, worden automatisch geannuleerd. 

![WHOIS record](./media/cdn-custom-ssl/whois-record.png)

DigiCert wordt ook een bevestigingsmail verzonden naar extra e-mailadressen. Als de WHOIS registrant informatie particulier is, controleert u of kunt u rechtstreeks vanuit een van de volgende adressen goedkeuren:

beheerder @&lt;your domain-name.com&gt;  
beheerder @&lt;your domain-name.com&gt;  
webbeheerder @&lt;your domain-name.com&gt;  
hostmaster @&lt;your domain-name.com&gt;  
postbeheerder @&lt;your domain-name.com&gt;  

U ontvangt een e-mailbericht over een paar minuten, vergelijkbaar met het volgende voorbeeld, waarin u de aanvraag goedkeuren. Als u een spamfilter gebruikt, voegt admin@digicert.com naar de goedgekeurde IP-adressen. Als u een e-mailbericht niet binnen 24 uur ontvangt, moet u contact op met Microsoft ondersteuning.
    
![Domein validatie e](./media/cdn-custom-ssl/domain-validation-email.png)

Wanneer u op de goedkeuringskoppeling klikt, wordt u omgeleid naar de volgende notatie voor de on line goedkeuring: 
    
![Formulier voor domein-validatie](./media/cdn-custom-ssl/domain-validation-form.png)

Volg de instructies op het formulier. hebt u twee opties voor verificatie:

- U kunt alle toekomstige orders geplaatst via dezelfde account voor het hoofddomein van dezelfde; goedkeuren bijvoorbeeld contoso.com. Deze aanpak wordt aanbevolen als u van plan bent extra aangepaste domeinen voor het hoofddomein van dezelfde kunnen worden toegevoegd.

- U kunt alleen de specifieke host-naam gebruikt in deze aanvraag goedkeuren. Er is aanvullende goedkeuring vereist voor volgende aanvragen.

Na de goedkeuring, wordt uw aangepaste domeinnaam in DigiCert toegevoegd aan de SAN-certificaat. Het certificaat is één jaar geldig en wordt automatische-vernieuwd voordat deze is verlopen.

### <a name="step-3-wait-for-propagation"></a>Stap 3: Wachten op doorgeven

Nadat de domeinnaam is gevalideerd, kan deze duren 6-8 uur voor de functie aangepast domein HTTPS worden geactiveerd. Wanneer het proces voltooid is, de status van de aangepaste HTTPS in de Azure portal is ingesteld op **ingeschakeld** en de bewerking vier stappen in het dialoogvenster aangepaste domein zijn gemarkeerd als voltooid. Uw aangepaste domein is nu gereed voor gebruik van HTTPS.

![Dialoogvenster HTTPS inschakelen](./media/cdn-custom-ssl/cdn-enable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Voortgang van bewerking

De volgende tabel toont de voortgang van de bewerking die wordt uitgevoerd wanneer u HTTPS inschakelen. Nadat u HTTPS inschakelt, worden vier stappen van de bewerking weergegeven in het dialoogvenster aangepast domein. Omdat elke stap geactiveerd wordt, is extra Substap details worden weergegeven onder de stap als deze beweegt. Niet alle deze substappen wordt uitgevoerd. Nadat een stap heeft voltooid, verschijnt een groen vinkje ernaast. 

| Bewerkingsstap | Details van de bewerking Substap | 
| --- | --- |
| 1 indienen aanvraag | Aanvraag verzenden |
| | Uw HTTPS-aanvraag wordt verzonden. |
| | Uw HHTPS-aanvraag is verzonden. |
| 2 domeinvalidatie | Domein automatisch gevalideerd als het CNAME toegewezen aan het CDN-eindpunt. Anders wordt een verzoek voor identiteitverificatie verzonden naar de e-mail die worden vermeld in uw domein registratierecord (WHOIS registrant). Controleer of het domein zo snel mogelijk. |
| | Uw domeineigendom is gevalideerd. |
| | Domein eigendom validatieaanvraag verlopen (de klant waarschijnlijk heeft niet gereageerd binnen 6 dagen). HTTPS wordt niet ingeschakeld op uw domein. * |
| | Domein eigendom validatie-aanvraag is geweigerd door de klant. HTTPS wordt niet ingeschakeld op uw domein. * |
| 3 inrichting van certificaten | De certificeringsinstantie verleent momenteel het certificaat dat nodig is om HTTPS in te schakelen in uw domein. |
| | Het certificaat is uitgegeven en wordt momenteel geïmplementeerd met CDN-netwerk. Dit kan enige tijd duren 6 uur. |
| | Het certificaat is geïmplementeerd in het CDN-netwerk. |
| 4 voltooid | HTTPS is ingeschakeld in uw domein. |

\* Dit bericht is alleen beschikbaar als een fout opgetreden. 


Als een fout optreedt voordat de aanvraag is verzonden, wordt het volgende foutbericht weergegeven:

<code>
We encountered an unexpected error while processing your HTTPS request. Please try again and contact support if the issue persists.
</code>

## <a name="disabling-https"></a>Disabling HTTPS

Wanneer u HTTPS op een aangepast domein hebt ingeschakeld, kunt u deze later uitschakelen. Als u wilt uitschakelen HTTPS, de volgende stappen uit:

### <a name="step-1-disable-the-feature"></a>Stap 1: De functie uitschakelen 

1. In de [Azure-portal](https://portal.azure.com), blader naar uw **Azure CDN Standard van Verizon** of **Azure CDN Premium van Verizon** CDN-profiel.

2. Klik op het eindpunt met uw aangepaste domein in de lijst met eindpunten.

3. Klik op het aangepaste domein waarvoor u wilt uitschakelen van HTTPS.

    ![Lijst met aangepaste domeinen](./media/cdn-custom-ssl/cdn-custom-domain-HTTPS-enabled.png)

4. Klik op **uit** als wilt uitschakelen HTTPS, klikt u vervolgens op **toepassen**.

    ![Dialoogvenster voor aangepaste HTTPS](./media/cdn-custom-ssl/cdn-disable-custom-ssl.png)

### <a name="step-2-wait-for-propagation"></a>Stap 2: Wachten op doorgeven

Nadat u het aangepaste domein HTTPS-functie is uitgeschakeld, kan deze kracht te laten tot 6-8 uur duren. Wanneer het proces voltooid is, de status van de aangepaste HTTPS in de Azure portal is ingesteld op **uitgeschakelde** en de stappen drie bewerking in het dialoogvenster aangepaste domein zijn gemarkeerd als voltooid. Uw aangepaste domein niet meer HTTPS kan gebruiken.

![Dialoogvenster HTTPS uitschakelen](./media/cdn-custom-ssl/cdn-disable-custom-ssl-complete.png)

### <a name="operation-progress"></a>Voortgang van bewerking

De volgende tabel toont de voortgang van de bewerking die deze gebeurtenis treedt op wanneer u HTTPS uitschakelt. Nadat u HTTPS uitschakelt, worden drie stappen van de bewerking in het aangepaste domein dialoogvenster weergegeven. Omdat elke stap geactiveerd wordt, is extra details worden weergegeven onder de stap. Nadat een stap heeft voltooid, verschijnt een groen vinkje ernaast. 

| Voortgang van bewerking | Bewerkingsdetails | 
| --- | --- |
| 1 indienen aanvraag | Uw aanvraag verzenden |
| 2 certificaat opheffen van inrichting | Het certificaat wordt verwijderd |
| 3 voltooid | Het certificaat is verwijderd |

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

1. *Wie is de certificaatprovider en type van het certificaat dat wordt gebruikt?*

    Microsoft gebruikt een namen met alternatieve onderwerp (SAN)-certificaat dat is opgegeven door DigiCert. Een SAN-certificaat kan meerdere FQDN-namen met één certificaat kunt beveiligen.

2. *Kan ik mijn speciaal certificaatarchief gebruiken?*
    
    Momenteel niet, maar het is in het overzicht.

3. *Wat gebeurt er als ik ontvang geen bevestigingsmail van het domein van DigiCert?*

    Als u een CNAME-item voor uw aangepaste domein die rechtstreeks naar de hostnaam van uw eindpunt verwijst hebt (en u de naam van het subdomein cdnverify niet gebruikt), ontvangt u geen een bevestigingsmail domein. Validatie wordt automatisch uitgevoerd. Anders, als u een CNAME-vermelding hebt en u een e-mailbericht niet binnen 24 uur ontvangen, contact op met Microsoft ondersteuning.

4. *Maakt gebruik van een SAN-certificaat minder veilig dan een speciaal certificaatarchief?*
    
    Een SAN-certificaat voldoet aan de dezelfde codering en beveiliging standaarden als een speciaal certificaatarchief. SHA-256 alle uitgegeven SSL-certificaten gebruiken voor uitgebreide beveiliging.

5. *Kan ik een aangepast domein HTTPS gebruiken met Azure CDN van Akamai?*

    Deze functie is momenteel alleen beschikbaar met Azure CDN van Verizon. Microsoft werkt deze functie met Azure CDN van Akamai in de komende maanden te ondersteunen.

6. *Moet ik een certificaat autoriteit autorisatie-record met mijn DNS-provider?*

    Nee, een certificaat autoriteit autorisatie-record is niet op dit moment vereist. Als u een hebt, moet deze DigiCert bevatten als een geldige CA.


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het instellen van een [aangepast domein op uw Azure CDN-eindpunt](./cdn-map-content-to-custom-domain.md)



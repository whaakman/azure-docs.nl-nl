---
title: On-premises wachtwoord terugschrijven integratie met Azure AD SSPR
description: Ophalen van cloud-wachtwoorden teruggeschreven naar on-premises AD infratstructure
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.openlocfilehash: 68823ffb7b274b8afc31de9d85bded3da853fcac
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54438062"
---
# <a name="what-is-password-writeback"></a>Wat is wachtwoord terugschrijven?

Een cloud-gebaseerde wachtwoord opnieuw instellen van hulpprogramma dat is erg handig, maar de meeste bedrijven hebben nog steeds een on-premises adreslijst waar hun gebruikers bestaan. Hoe heeft Microsoft ondersteuning houden traditionele on-premises Active Directory (AD) gesynchroniseerd met het wachtwoord te wijzigen in de cloud? Wachtwoord terugschrijven is een functie is ingeschakeld met [Azure AD Connect](../hybrid/whatis-hybrid-identity.md) die hierbij worden wachtwoordwijzigingen in de cloud worden teruggeschreven naar een bestaande on-premises adreslijst in realtime.

Wachtwoord terugschrijven wordt ondersteund in omgevingen die gebruikmaken van:

* [Active Directory Federation Services](../hybrid/how-to-connect-fed-management.md)
* [Synchronisatie van wachtwoord-hashes](../hybrid/how-to-connect-password-hash-synchronization.md)
* [Pass-through-verificatie](../hybrid/how-to-connect-pta.md)

> [!WARNING]
> Wachtwoord terugschrijven wordt niet meer voor klanten die werken met Azure AD Connect-versies 1.0.8641.0 en oudere wanneer de [Azure Access Control service (ACS) op 7 November 2018 buiten gebruik is gesteld](../develop/active-directory-acs-migration.md). Azure AD Connect-versies 1.0.8641.0 en ouder wordt geen bestanden meer worden het terugschrijven van wachtwoorden op dat moment omdat ze afhankelijk van ACS voor deze functionaliteit zijn.
>
> Om te voorkomen dat een onderbreking in de service, een upgrade uitvoeren van een eerdere versie van Azure AD Connect naar een nieuwere versie, Zie het artikel [Azure AD Connect: Upgrade van een vorige versie naar de nieuwste versie](../hybrid/how-to-upgrade-previous-version.md)
>

Wachtwoord terugschrijven biedt:

* **Afdwinging van on-premises Active Directory-wachtwoordbeleid**: Wanneer een gebruiker hun wachtwoord opnieuw instellen, wordt deze gecontroleerd om te controleren of deze voldoet aan uw beleid on-premises Active Directory voordat u deze doorvoert naar die map. Deze beoordeling omvat de geschiedenis, complexiteit, leeftijd, wachtwoordfilters en andere wachtwoordbeperkingen die u hebt gedefinieerd in de lokale Active Directory controleren.
* **Nul vertraging feedback**: Wachtwoord terugschrijven is een synchrone bewerking. Uw gebruikers zijn onmiddellijk een melding als hun wachtwoord niet voldoet aan het beleid kan niet worden opnieuw ingesteld of gewijzigd voor een bepaalde reden.
* **Ondersteunt het wachtwoord wordt gewijzigd in het toegangsvenster en de Office 365**: Wanneer federatieve of wachtwoord-hash gesynchroniseerd gebruikers tot het wijzigen van het wachtwoord is verlopen of niet-verlopen komen, deze wachtwoorden worden teruggeschreven naar uw lokale Active Directory-omgeving.
* **Biedt ondersteuning voor het terugschrijven van wachtwoorden wanneer een beheerder stelt ze opnieuw vanuit de Azure-portal**: Wanneer een beheerder stelt het wachtwoord van een gebruiker in de [Azure-portal](https://portal.azure.com), als die gebruiker is gefedereerd of wachtwoord-hash wordt gesynchroniseerd, het wachtwoord is teruggeschreven naar on-premises. Deze functionaliteit is momenteel niet ondersteund in de Office-beheerportal.
* **Vereist geen inkomende firewallregels**: Wachtwoord terugschrijven wordt een Azure Service Bus relay gebruikt als een onderliggende communicatiekanaal. Alle communicatie is via poort 443 uitgaande.

> [!Note]
> Gebruikersaccounts die zijn opgeslagen in beveiligde groepen in on-premises Active Directory kunnen niet worden gebruikt met het terugschrijven van wachtwoorden. Administrator-accounts die zijn opgeslagen in beveiligde groepen in de on-premises AD kan worden gebruikt met het terugschrijven van wachtwoorden. Zie voor meer informatie over beveiligde groepen [beveiligde accounts en groepen in Active Directory](https://technet.microsoft.com/library/dn535499.aspx).
>

## <a name="licensing-requirements-for-password-writeback"></a>Licentievereisten voor wachtwoord terugschrijven

**Selfservice wachtwoord opnieuw instellen/wijzigen/ontgrendelen met on-premises Write-back is een premium-functie van Azure AD**. Zie voor meer informatie over de licentieverlening voor de [Azure Active Directory-site prijzen](https://azure.microsoft.com/pricing/details/active-directory/).

Voor het gebruik van het terugschrijven van wachtwoorden, moet u een van de volgende licenties zijn toegewezen op uw tenant hebben:

* Azure AD Premium P1
* Azure AD Premium P2
* Enterprise Mobility + Security E3 of A3
* Enterprise Mobility + Security E5 of A5
* Microsoft 365 E3 of A3
* Microsoft 365 E5 of A5
* Microsoft 365 F1
* Microsoft 365 Business

> [!WARNING]
> Zelfstandige Office 365-abonnementen licentieverlening *bieden geen ondersteuning voor 'Selfservice wachtwoord opnieuw instellen/wijzigen/ontgrendelen met on-premises Write-back van'* en vereisen dat u met een van de voorgaande plannen voor deze functionaliteit te werken.
>

## <a name="how-password-writeback-works"></a>De werking van wachtwoord terugschrijven

Wanneer een federatieve of wachtwoord-hash gesynchroniseerd pogingen van gebruikers opnieuw instellen of wijzigen van het wachtwoord in de cloud, wordt de volgende acties uitgevoerd:

1. Een controle wordt uitgevoerd om te zien welke type wachtwoord op de gebruiker heeft. Als het wachtwoord on-premises worden beheerd is:
   * Een controle wordt uitgevoerd om te zien of de Write-back-service actief en werkend. Als dit het geval is, wordt de gebruiker kunt doorgaan.
   * Als de Write-back-service niet actief is, wordt de gebruiker geïnformeerd dat hun wachtwoord nu kan niet worden teruggezet.
1. Vervolgens de gebruiker de juiste verificatie-gates is geslaagd en bereikt de **wachtwoord opnieuw instellen** pagina.
1. De gebruiker selecteert een nieuw wachtwoord en bevestigt het.
1. Wanneer de gebruiker selecteert **indienen**, het leesbare wachtwoord is versleuteld met een symmetrische sleutel die is gemaakt tijdens de installatieprocedure voor het terugschrijven van wachtwoorden.
1. Het versleutelde wachtwoord is opgenomen in een nettolading wordt verzonden via een HTTPS-kanaal aan uw tenant-specifieke service bus-omleiding (die is ingesteld voor u tijdens de installatieprocedure voor het terugschrijven van wachtwoorden). Deze relay wordt beveiligd door een willekeurig gegenereerd wachtwoord dat alleen de installatie van uw on-premises kent.
1. Nadat het bericht heeft bereikt van de servicebus, wordt het eindpunt opnieuw instellen van wachtwoorden automatisch ontwaakt en ziet dat er een aanvraag in behandeling.
1. De gebruiker zoekt de service vervolgens met behulp van de cloud ankerkenmerk. Voor deze zoekactie te voltooien:

   * Het gebruikersobject moet bestaan in het Active Directory-connectorgebied.
   * Het gebruikersobject moet worden gekoppeld aan het bijbehorende (MV) metaverse-object.
   * Het gebruikersobject moet worden gekoppeld aan het bijbehorende Azure Active Directory-connector-object.
   * De koppeling van de Active Directory-connector-object naar de MV ze beschikken over de synchronisatieregel `Microsoft.InfromADUserAccountEnabled.xxx` op de koppeling.
   
   De synchronisatie-engine wordt gebruikt als de oproep afkomstig van de cloud is, de **waarde voor cloudAnchor** kenmerk om te controleren of de Azure Active Directory-connector space-object. Het volgt de koppeling op naar het MV-object, en volgt de koppeling op naar het Active Directory-object. Omdat er meerdere Active Directory-objecten (meerdere forests) voor dezelfde gebruiker zijn, de synchronisatie-engine is afhankelijk van de `Microsoft.InfromADUserAccountEnabled.xxx` koppeling naar het juiste abonnement kiezen.

   > [!Note]
   > Als gevolg van deze logica voor wachtwoord moet terugschrijven in om te werken van Azure AD Connect kunnen communiceren met de primaire domeincontroller (PDC) emulator. Als u deze handmatig inschakelt moet, kunt u Azure AD Connect verbinding met de PDC-emulator. Met de rechtermuisknop op de **eigenschappen** van de Active Directory-synchronisatieconnector, selecteert u vervolgens **mappartities configureren**. Zoek daar de **verbindingsinstellingen** sectie en selecteer het vak met de titel **gebruik alleen domeincontrollers**. Zelfs als de gewenste domeincontroller niet een PDC-emulator is, probeert Azure AD Connect om verbinding met de PDC voor het terugschrijven van wachtwoorden te maken.

1. Nadat de gebruiker is-account gevonden, wordt een poging tot opnieuw instellen van het wachtwoord rechtstreeks in de juiste Active Directory-forest wordt uitgevoerd.
1. Als het wachtwoord set-bewerking geslaagd is, wordt de gebruiker gemeld dat hun wachtwoord is gewijzigd.
   > [!NOTE]
   > Als de wachtwoord-hash van de gebruiker is gesynchroniseerd met Azure AD met behulp van wachtwoord-hashsynchronisatie, is er een kans dat de on-premises Wachtwoordbeleid toepassen zwakkere dan het wachtwoordbeleid van de cloud is. In dit geval wordt de on-premises-beleid afgedwongen. Dit beleid zorgt ervoor dat uw on-premises-beleid wordt afgedwongen in de cloud, ongeacht als u wachtwoord-hashsynchronisatie of Federatie gebruiken voor eenmalige aanmelding.
   >

1. Als het wachtwoord bewerking mislukt instelt, is een fout wordt gevraagd om de gebruiker en probeer het opnieuw. De bewerking kan mislukken omdat:
   * De service is niet beschikbaar.
   * Het wachtwoord dat ze geselecteerd niet voldoet aan beleidsregels van de organisatie.
   * Kan de gebruiker in de lokale Active Directory vinden.

    De foutberichten bieden richtlijnen aan gebruikers zodat ze proberen kunnen op te lossen zonder tussenkomst van de beheerder.

## <a name="password-writeback-security"></a>Wachtwoord terugschrijven beveiliging

Wachtwoord terugschrijven is een zeer veilige service. Om te zorgen dat uw gegevens worden beschermd, wordt een beveiligingsmodel met vier lagen ingeschakeld als de volgende stappen wordt beschreven:

* **Tenant-specifieke service bus relay**
   * Bij het instellen van de service, is een tenant-specifieke service bus-omleiding ingesteld dat wordt beveiligd door een willekeurig gegenereerd sterk wachtwoord dat Microsoft nooit toegang tot heeft.
* **De versleutelingssleutel vergrendeld, cryptografisch sterk wachtwoord**
   * Nadat de service bus relay is gemaakt, wordt er een sterke symmetrische sleutel gemaakt die wordt gebruikt voor het versleutelen van het wachtwoord als het gaat via de kabel. Deze sleutel is alleen aanwezig in de geheime store van uw bedrijf in de cloud, die sterk is vergrendeld en gecontroleerd, net als elk ander wachtwoord in de map.
* **Branche standaard Transport Layer Security (TLS)**
   1. Wanneer een wachtwoord opnieuw instellen of wijzigen bewerking wordt uitgevoerd in de cloud, het leesbare wachtwoord is versleuteld met de openbare sleutel.
   1. Het versleutelde wachtwoord wordt geplaatst in een HTTPS-bericht dat wordt verzonden via een versleuteld kanaal met behulp van Microsoft-SSL-certificaten naar uw service bus relay.
   1. Nadat het bericht wordt ontvangen in de servicebus, wordt uw on-premises-agent wordt weer actief en wordt geverifieerd bij de servicebus met behulp van het wachtwoord dat eerder is gegenereerd.
   1. De on-premises agent neemt het gecodeerde bericht en ontsleutelt deze met behulp van de persoonlijke sleutel.
   1. De on-premises-agent probeert het wachtwoord via de API van AD DS SetPassword in te stellen. Deze stap is wat afdwinging van uw Active Directory on-premises Wachtwoordbeleid toepassen (zoals de complexiteit, leeftijd, geschiedenis en filters) is toegestaan in de cloud.
* **Bericht verloopbeleid voor**
   * Als het bericht bevindt zich in servicebus, omdat uw on-premises service niet actief is, een time-out optreedt en wordt verwijderd na enkele minuten. De time-out en het verwijderen van het bericht verhoogt de beveiliging nog verder.

### <a name="password-writeback-encryption-details"></a>Versleutelingsgegevens voor write-back van wachtwoord

Nadat een gebruiker een wachtwoord opnieuw instellen is ingediend, gaat de aanvraag opnieuw instellen via verschillende stappen voor het versleutelen voordat ze worden ontvangen in uw on-premises omgeving. Deze stappen voor het versleutelen Zorg ervoor dat de service met maximale beschikbaarheid en beveiliging. Ze worden als volgt beschreven:

* **Stap 1: Wachtwoordversleuteling met 2048-bits RSA-sleutel**: Nadat een gebruiker een wachtwoord worden teruggeschreven naar on-premises verzendt, wordt het opgegeven wachtwoord zelf worden versleuteld met een 2048-bits RSA-sleutel.
* **Stap 2: Versleuteling met AES-GCM-pakket op**: Het volledige pakket, het wachtwoord en de vereiste metagegevens is versleuteld met AES-GCM. Deze versleuteling zorgt ervoor dat niemand met directe toegang naar de onderliggende service bus-kanaal weergeven of knoeien met de inhoud.
* **Stap 3: Alle communicatie vindt plaats via TLS/SSL**: Alle communicatie met Service Bus gebeurt in een SSL/TLS-kanaal. Deze versleuteling voor beveiliging van de inhoud van een niet-geautoriseerde derde partij.
* **Automatische implementatie van de sleutel in elke zes maanden**: Alle sleutels herstellen via elke zes maanden, of telkens wanneer het terugschrijven van wachtwoorden is uitgeschakeld en vervolgens weer wordt ingeschakeld op de Azure AD Connect, om ervoor te zorgen voor maximale service en veiligheid.

### <a name="password-writeback-bandwidth-usage"></a>Bandbreedtegebruik van wachtwoord terugschrijven

Wachtwoord terugschrijven is een service met een lage bandbreedte die alleen aanvragen naar de on-premises-agent in de volgende omstandigheden verzendt:

* Twee berichten worden verzonden wanneer de functie is ingeschakeld of uitgeschakeld met Azure AD Connect.
* Een bericht wordt elke vijf minuten als een heartbeat van de service voor verzonden als de service wordt uitgevoerd.
* Twee berichten worden verzonden op elk moment een nieuw wachtwoord wordt ingediend:
   * Het eerste bericht is een verzoek om de bewerking niet uitvoeren.
   * Het tweede bericht bevat het resultaat van de bewerking en wordt verzonden in de volgende omstandigheden:
      * Telkens wanneer een nieuw wachtwoord wordt ingediend bij de self-service voor wachtwoord opnieuw instellen van een gebruiker.
      * Telkens wanneer een nieuw wachtwoord wordt ingediend bij een wachtwoordwijziging van gebruiker.
      * Telkens wanneer een nieuw wachtwoord wordt ingediend bij een beheerder geïnitieerde gebruikerswachtwoord opnieuw instellen (alleen vanaf de Azure admin-Portal).

#### <a name="message-size-and-bandwidth-considerations"></a>Overwegingen voor de grootte en bandbreedte van bericht

De grootte van elk van de eerder beschreven bericht is meestal dan 1 KB. Zelfs bij extreme belasting, is de service voor het terugschrijven van wachtwoord zelf enkele kilobits per seconde van de bandbreedte verbruikt. Omdat elk bericht wordt verzonden in realtime, is alleen in indien nodig door een bewerking voor bijwerken, en omdat de berichtgrootte zo klein is, is het bandbreedtegebruik van de mogelijkheid Write-back te klein om een merkbare impact heeft.

## <a name="supported-writeback-operations"></a>Write-back van ondersteunde bewerkingen

Wachtwoorden worden teruggeschreven in de volgende situaties:

* **Ondersteunde eindgebruikers**
   * Alle eindgebruikers Self-servicegebruikers vrijwillige bewerking wachtwoord wijzigen
   * Alle eindgebruikers Self-servicegebruikers force wijzigen wachtwoord bewerking, bijvoorbeeld: wachtwoord verloopt
   * Een eindgebruiker Self-service voor wachtwoord opnieuw instellen dat afkomstig van is de [portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com)
* **De beheerder van de ondersteunde bewerkingen**
   * Een beheerder selfservice vrijwillige bewerking wachtwoord wijzigen
   * Een beheerder selfservice-force wijzigen wachtwoord bewerking, bijvoorbeeld: wachtwoord verloopt
   * Een beheerder van self-service voor wachtwoord opnieuw instellen dat afkomstig van is de [portal voor wachtwoordherstel](https://passwordreset.microsoftonline.com)
   * Een eindgebruiker door de beheerder voor wachtwoord opnieuw instellen van de [Azure-portal](https://portal.azure.com)

## <a name="unsupported-writeback-operations"></a>Write-back van niet-ondersteunde bewerkingen

Wachtwoorden zijn *niet* teruggeschreven in een van de volgende situaties:

* **Bewerkingen voor niet-ondersteunde eindgebruikers**
   * Een eindgebruiker hun eigen wachtwoord opnieuw instellen met behulp van PowerShell-versie 1, versie 2 of de Azure AD Graph API
* **Niet-ondersteunde bewerkingen**
   * Een eindgebruiker door de beheerder voor wachtwoord opnieuw instellen van de [Office-beheerportal](https://portal.office.com)
   * Een eindgebruiker door de beheerder voor wachtwoord opnieuw instellen van de PowerShell-versie 1, versie 2 of de Azure AD Graph API

## <a name="next-steps"></a>Volgende stappen

Wachtwoord terugschrijven inschakelen met de zelfstudie: [Wachtwoord terugschrijven inschakelen](tutorial-enable-writeback.md)

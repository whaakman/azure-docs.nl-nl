---
title: In het Edge-gegevensbeveiliging | Microsoft Docs
description: Beschrijving van de beveiliging en privacy-functies die uw gegevens in het Edge-apparaat, -service en -gegevens on-premises en in de cloud beveiligen.
services: Data Box Edge
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 04/02/2019
ms.author: alkohli
ms.openlocfilehash: de737f20147e8208dd18388eedcac11583c8cb97
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58891800"
---
# <a name="data-box-edge-security-and-data-protection"></a>Gegevens in het Edge-beveiliging en gegevensbescherming

Beveiliging is van groot belang bij het kiezen van een nieuwe technologie, met name als de technologie wordt gebruikt met vertrouwelijke of geheime gegevens. Microsoft Azure Data Box Edge oplossing helpt ervoor te zorgen dat alleen geautoriseerde entiteiten kunnen weergeven, wijzigen of verwijderen van uw gegevens.

Dit artikel beschrijft de gegevens in het Edge-beveiligingsfuncties die helpen beschermen van elk van de onderdelen van de oplossing en de gegevens die zijn opgeslagen op deze.

De oplossing Azure Data Box Edge bestaat uit vier hoofdonderdelen die met elkaar communiceren:

- **Data Box Edge / gegevens in het Gateway-service die wordt gehost in Azure** – de management-resource die u gebruiken voor het maken van de volgorde van de apparaten, het apparaat configureren en vervolgens de volgorde voor voltooiing bijhouden.
- **Gegevens in het Edge-apparaat** – de overdrachtsapparaat dat u uw on-premises gegevens importeren in Azure wordt verzonden.
- **Clients /-hosts die zijn verbonden met het apparaat** – de clients in uw infrastructuur die verbinding maken met de gegevens in het Edge-apparaat en die gegevens bevatten die moet worden beveiligd.
- **Cloudopslag** – De locatie in de Azure-cloud waar gegevens worden opgeslagen. Deze locatie is meestal het opslagaccount dat is gekoppeld aan de gegevens in het Edge-resource die u hebt gemaakt.


## <a name="data-box-edgedata-box-gateway-service-protection"></a>Gegevensbeveiliging in Edge/Data Box-Gateway-service

De gegevensgateway in Edge/de Data Box-service is een management-service die wordt gehost in Microsoft Azure. De service wordt gebruikt voor het configureren en beheren van het apparaat.

- Toegang tot de gegevens in Edge/gegevens in het Gateway-service is vereist uw organisatie een Enterprise Agreement (EA) of een Cloud Solution Provider (CSP)-abonnement hebben. Ga voor meer informatie naar [zich aanmelden voor een Azure-abonnement](https://azure.microsoft.com/resources/videos/sign-up-for-microsoft-azure/)!
- Omdat uw management-service wordt gehost in Azure, wordt deze wordt beveiligd door de functies van Azure-beveiliging. Ga naar het [Vertrouwenscentrum van Microsoft Azure](https://azure.microsoft.com/support/trust-center/security/) voor meer informatie over de beveiligingsfuncties die worden geleverd door Microsoft Azure.
- Voor de SDK-beheerbewerkingen, versleutelingssleutel is beschikbaar voor uw Data Box-Edge / Data Box Gateway resource onder **apparaateigenschappen**. U vindt de versleutelingssleutel alleen als u machtigingen voor de Resource Graph-API hebt.

## <a name="data-box-edge-device-protection"></a>Bescherming van gegevens in het Edge-apparaat

De gegevens in het Edge-apparaat is een on-premises-apparaat waarmee de gegevens transformeren met lokale verwerking en vervolgens te verzenden naar Azure. Uw apparaat:

- Moet een activeringscode voor toegang tot de gegevens in Edge/gegevens in het Gateway-service.
- Wordt beveiligd op altijd door het wachtwoord van een apparaat.
- Een apparaat vergrendeld is. Het apparaat BMC en BIOS wachtwoord zijn beveiligd met beperkte gebruikerstoegang voor het BIOS.
- Beveiligd opstarten is ingeschakeld.
- Wordt uitgevoerd Windows Defender Device Guard. Device Guard kunt u alleen vertrouwde toepassingen die u in uw code-integriteitsbeleid definieert uit te voeren.
- Heeft een sleutel in de omslag die kan worden gebruikt om het apparaat te vergrendelen. We aanbevelen die na het configureren van het apparaat, opent de dekking. Ga naar de sleutel en klikt u vervolgens de dekking om te voorkomen dat onbevoegde toegang om gegevensschijven zich aan de voorzijde van het apparaat te vergrendelen.

### <a name="protect-the-device-via-activation-key"></a>Het apparaat via de activeringscode beveiligen

Alleen een geautoriseerde gegevens in het Edge-apparaat mag deelnemen aan de Data Box Edge/gegevens in het Gateway-service die u hebt gemaakt in uw Azure-abonnement. Als u wilt toestaan dat een apparaat, moet u een activeringscode te activeren van het apparaat met de gegevensgateway in Edge/de Data Box-service. Ga voor meer informatie naar [ophalen van een activeringscode](data-box-edge-deploy-prep.md#get-the-activation-key).

De activeringscode die u gebruikt:

- Is de sleutel van een Azure Active Directory (AAD) gebaseerde verificatie.
- Verloopt na drie dagen.
- Na apparaatactivering van het wordt niet gebruikt.
 
Nadat een apparaat wordt geactiveerd, worden tokens gebruikt om te communiceren met Microsoft Azure.

### <a name="protect-the-device-via-password"></a>Het apparaat via wachtwoord beveiligen

Wachtwoorden ervoor te zorgen dat uw gegevens toegankelijk voor alleen gemachtigde gebruikers is. Gegevens in het Edge- en Data Box Gateway apparaten opstart in een vergrendelde status.

U kunt:

- Verbinding maken met de lokale web-UI van het apparaat via een browser en geef vervolgens een wachtwoord aan te melden bij het apparaat.
- Op afstand verbinding maken met de PowerShell-interface van het apparaat via HTTP. Extern beheer is standaard ingeschakeld. Vervolgens kunt u het wachtwoord van het apparaat aan te melden bij het apparaat opgeven. Ga voor meer informatie naar [verbinding maken met uw gegevens in het Edge-apparaat op afstand](data-box-edge-connect-powershell-interface.md#connect-to-the-powershell-interface).

Houd rekening met de volgende aanbevolen procedures:

- De gegevens in het Edge-service kan de bestaande wachtwoorden niet ophalen: het alleen kunt herstellen via Azure portal. Het is raadzaam dat u alle wachtwoorden opslaan op een veilige plaats, zodat u geen hebt een wachtwoord opnieuw instellen als deze is vergeten. Als u een wachtwoord opnieuw instelt, zorg er dan voor dat alle gebruikers waarschuwen voordat u het opnieuw instellen.
- Gebruik de lokale web-UI te [het wachtwoord wijzigen](data-box-gateway-manage-access-power-connectivity-mode.md#manage-device-access). Als u het wachtwoord wijzigt, zorg er dan voor dat alle externe toegang-gebruikers een melding ontvangen zodat ze zich niet een aanmelding mislukt.
- U kunt de Windows PowerShell-interface van uw apparaat op afstand benaderen via HTTP. Als een aanbevolen beveiligingsprocedure, moet u HTTP alleen in vertrouwde netwerken.
- Zorg ervoor dat apparaat wachtwoorden sterke en goed beveiligd. Ga als volgt de [aanbevolen procedures voor wachtwoorden](https://docs.microsoft.com/azure/security/azure-security-identity-management-best-practices#enable-password-management).

## <a name="protect-the-data"></a>De gegevens te beveiligen

Deze sectie beschrijft de gegevens in het Edge-beveiligingsfuncties waarmee de gegevens die onderweg zijn en opgeslagen gegevens worden beveiligd.

### <a name="protect-data-at-rest"></a>Gegevens in rust beveiligen

Voor de data-at-rest:

- Voor de data-at-rest, gegevens in Edge BitLocker XTS-AES-256-codering gebruikt om de lokale gegevens te beveiligen.
- De gegevens die zich in shares bevinden, is de toegang tot de shares beperkt.

    - Voor SMB-clients die toegang hebben tot de gegevens delen, moeten ze de referenties van de gebruiker die is gekoppeld aan de share. Deze referenties worden gedefinieerd op het moment dat de share gemaakt.
    - Voor NFS-clients die toegang hebben tot de shares, moeten de IP-adressen van de clients op het moment dat de share gemaakt worden toegevoegd.


### <a name="protect-data-in-flight"></a>Beveiligen van gegevens tijdens de overdracht

Voor de gegevens-in-vlucht:

- Voor gegevens die tussen het apparaat en Azure gaat, wordt standaard TLS 1.2 gebruikt. Er is er geen terugval naar TLS 1.1 en eerdere versies. De communicatie met de agent wordt geblokkeerd als TLS 1.2 wordt niet ondersteund. De TLS 1.2 is ook vereist voor de portal en SDK-beheerbewerkingen.
- Wanneer de clients toegang het apparaat via de lokale webgebruikersinterface in een browser tot, wordt standaard TLS 1.2 wordt gebruikt als de standaard veilig protocol.

    - De aanbevolen procedure is het configureren van uw browser voor het gebruik van TLS 1.2.
    - Als de browser biedt geen ondersteuning voor TLS 1.2, kunt u TLS 1.1 of TLS 1.0.
- Als u wilt de gegevens te beschermen bij het kopiëren van uw gegevensservers, wordt u aangeraden dat u SMB 3.0 met codering gebruiken.

### <a name="protect-data-via-storage-accounts"></a>Bescherm gegevens via de storage-accounts

Uw apparaat is gekoppeld aan een opslagaccount dat wordt gebruikt als een doel voor uw gegevens in Azure. Toegang tot het opslagaccount dat wordt bepaald door het abonnement en twee 512-bits opslag toegang tot sleutels die zijn gekoppeld aan dit account.

Een van de sleutels wordt gebruikt voor verificatie wanneer de gegevens in het Edge-apparaat toegang heeft tot het opslagaccount. De andere sleutel is ondergebracht in reserveren, zodat u kunt de sleutels periodiek draaien.

Uit veiligheidsoverwegingen vereisen veel datacenters rouleren van de sleutel. U wordt aangeraden dat u deze aanbevolen procedures voor sleutelroulatie volgt:

- De sleutel van uw opslagaccount is vergelijkbaar met het hoofdwachtwoord voor uw opslagaccount. Uw accountsleutel zorgvuldig te beschermen. Niet distribueren van het wachtwoord aan andere gebruikers, hard foutcode of sla deze ergens als tekst zonder opmaak die toegankelijk is voor anderen.
- [Accountsleutel](../storage/common/storage-account-manage.md#regenerate-access-keys) met behulp van de Azure-portal als u denkt het dat mogelijk zijn aangetast.
- Draaien en vervolgens [uw opslagaccountsleutels synchroniseren](data-box-gateway-manage-shares.md#sync-storage-keys) regelmatig om ervoor te zorgen dat uw storage-account niet is geopend door onbevoegde gebruikers.
- Uw Azure-beheerder moet periodiek, wijzigen of de primaire of secundaire sleutel opnieuw genereren met behulp van de sectie voor opslag van de Azure-portal voor rechtstreekse toegang tot het opslagaccount.


## <a name="manage-personal-information"></a>Persoonlijke gegevens beheren

De Data Box-Edge / gegevens in het Gateway-service verzamelt persoonlijke gegevens in de volgende belangrijke gevallen:

- **Ordergegevens** – Zodra de order is gemaakt, worden het verzendadres, het e-mailadres en de contactgegevens van gebruikers in de Azure-portal opgeslagen. De opgeslagen informatie omvat:
  - Naam van contactpersoon
  - Telefoonnummer
  - Email
  - Adres
  - Plaats
  - Postcode
  - Status
  - Land/Provincie/Regio
  - Volgnummer van verzending

    Details van de order worden versleuteld en opgeslagen in de service. De informatie wordt door de service behouden totdat u de resource of order expliciet verwijdert. Bovendien wordt de verwijdering van de resource en de bijbehorende order geblokkeerd vanaf het moment dat het apparaat totdat het apparaat naar Microsoft wordt wordt verzonden.

- **Verzendadres** – nadat de order is geplaatst, Data Box-service het verzendadres biedt voor providers van derden zoals Getotaliseerd.

- **Gebruikers delen** -gebruikers op uw apparaat kunnen ook toegang tot de gegevens die zich bevinden op de shares. Een lijst met gebruikers die toegang de sharegegevens tot krijgen wordt weergegeven en kan worden weergegeven. Deze lijst wordt ook verwijderd wanneer de shares worden verwijderd. Volg de stappen in de lijst van gebruikers die toegang tot of verwijderen van een share wilt weergeven, [beheren van bestandsshares aan de rand van gegevens in het](data-box-gateway-manage-shares.md).

Bekijk het Microsoft-privacybeleid in het [Vertrouwenscentrum](https://www.microsoft.com/trustcenter) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

[Uw gegevens in het Edge-apparaat implementeren](data-box-edge-deploy-prep.md).


---
title: Beveiligen van persoonlijke gegevens die onderweg met versleuteling in Azure | Microsoft Docs
description: Met behulp van versleuteling persoonlijke gegevens te beveiligen in Azure
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 461ddfda796bfe6639e27f6c4cd53e82b4e397d1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a>Azure versleutelingstechnologieën: beveiligen van persoonlijke gegevens die onderweg met versleuteling

In dit artikel helpt u begrijpen en gebruiken van Azure versleutelingstechnologieën beveiligen van gegevens die worden verzonden. 

De privacy van persoonlijke gegevens te beveiligen als ze via het netwerk is een essentieel onderdeel van een meerlaagse beveiliging van defense-in-depth-strategie. Versleuteling onderweg is ontworpen om te voorkomen dat een aanvaller die gegevensoverdrachten te geven of de gegevens kunnen worden onderschept.

## <a name="scenario"></a>Scenario

Een bedrijf grote cruise, gevestigd in de Verenigde Staten, aanvullende bewerkingen voor het bieden van routes in de Middellandse, Adriatische, Baltische veiligheid ook Florida. Ter ondersteuning van deze inspanningen is die is verkregen meerdere kleinere cruise regels op basis van Italië, Duitsland, Denemarken en het Verenigd Koninkrijk 

Het bedrijf gebruikmaakt van Microsoft Azure voor het opslaan van bedrijfsgegevens in de cloud. Dit omvat persoonlijk gegevens zoals namen, adressen, telefoonnummers en creditcardgegevens van globale klantendatabase. Dit omvat ook traditionele Human Resource informatie zoals adressen, telefoonnummers, BTW-id's en andere informatie over de werknemers van het bedrijf op alle locaties. De regel cruise onderhoudt ook een grote database van derden en loyaliteit programma leden met persoonlijke gegevens voor de relaties met de huidige en eerdere klanten bijhouden.

Persoonlijke gegevens van klanten is ingevoerd in de database uit van het bedrijf externe kantoren en reizen agents over de hele wereld. Documenten met klantgegevens worden overgebracht via het netwerk naar Azure-opslag.

## <a name="problem-statement"></a>Probleemformulering

Het bedrijf moet de privacy van klanten en werknemers persoonlijke gegevens beveiligen terwijl het onderweg en naar Azure-services.

## <a name="company-goal"></a>Bedrijf-doel

Het doel van het bedrijf om ervoor te zorgen dat de persoonlijke gegevens van de harde schijf worden versleuteld. Als niet-geautoriseerde personen de persoonlijke gegevens buiten de schijf Intercept, moet deze in een formulier dat het onleesbaar verschijnt. Versleuteling toepassen, moet eenvoudig of volledig transparant voor gebruikers en beheerders.

## <a name="solutions"></a>Oplossingen

Azure-services bieden meerdere hulpprogramma's en technologieën voor het persoonlijke gegevens onderweg te beschermen.

### <a name="azure-storage"></a>Azure Storage

Gegevens die zijn opgeslagen in de cloud moet van de client, die zich fysiek overal ter wereld, naar het Azure Datacenter gaan. Wanneer er gegevens worden opgehaald door gebruikers, overdracht opnieuw in de tegengestelde richting. Gegevens die via het openbare Internet onderweg is altijd risico worden onderschept door kwaadwillende personen. Het is belangrijk om de privacy van persoonlijke gegevens beschermen met behulp van transportniveau versleuteling te beveiligen als u deze verplaatst tussen locaties.

Het HTTPS-protocol biedt een kanaal beveiligde, gecodeerde communicatie via Internet. HTTPS moet worden gebruikt voor toegang tot objecten in Azure Storage en bij het aanroepen van REST-API's. U gebruik van het HTTPS-protocol afdwingen wanneer u [Shared Access Signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) te delegeren van toegang tot Azure Storage-objecten. Er zijn twee soorten SAS: Service-SAS en Account-SAS.

#### <a name="how-do-i-construct-a-service-sas"></a>Hoe ik een Service-SAS maken?

Een Service-SAS delegeert toegang tot een resource in slechts één van de storage-services (blob, queue, tabel of file-service). Kan een Service-SAS, het volgende doen:

1. Het veld ondertekende versie opgeven

2. Geef de ondertekende Resource (Blob en File-Service)

3. Geef queryparameters op om te overschrijven antwoordheaders (Blob-Service en File-Service)

4. Geef de naam van de tabel (alleen voor Tabelservice)

5. Geef het beleid voor toegang

6. Geef het geldigheidsinterval van de handtekening

8. Machtigingen opgeven

9. IP-adres of IP-bereik opgeven

10. Geef de HTTP-Protocol

11. Tabel toegang adresbereiken opgeven

12. Hiermee geeft u de ondertekende id

13. Geef de handtekening

Zie voor meer instructies, [samenstellen van een Service-SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).

#### <a name="how-do-i-construct-an-account-sas"></a>Hoe ik een SAS-Account maken?

Een Account-SAS delegeert toegang tot bronnen in een of meer van de storage-services. U kunt ook toegang tot het lezen, schrijven en verwijderen van bewerkingen delegeren voor blobcontainers, tabellen, wachtrijen en bestandsshares die niet zijn toegestaan bij een service-SAS. Bouw van een SAS-Account is vergelijkbaar met die van een Service-SAS. Zie voor gedetailleerde instructies [samenstellen van een SAS-Account.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a>Hoe ik HTTPS afdwingen bij het aanroepen van REST-API's?

Het gebruik van HTTPS afdwingen bij het aanroepen van REST-API's voor toegang tot objecten in de storage-accounts, kunt u beveiligde overdracht vereist voor het opslagaccount. 

1. Selecteer in de Azure-portal **Storage-Account maken**, of Selecteer voor een bestaand opslagaccount **instellingen** en vervolgens **configuratie**.

2. Onder **beveiligde overdracht vereist**, selecteer **ingeschakeld**.

![Een opslagaccount maken](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

Voor instructies gedetailleerde, met inbegrip van beveiligde overdracht vereist programmatisch inschakelen, Raadpleeg [vereisen Secure Transfer](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a>Versleutelen gegevens in Azure File Storage?

Voor het versleutelen van gegevens tijdens de overdracht met [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), kunt u SMB 3.x met Windows 8, 8.1 en 10 en Windows Server 2012 R2 en Windows Server 2016. Wanneer u de service Azure-bestanden gebruikt, wordt er een verbinding zonder versleuteling mislukt wanneer "Veilig vereiste gegevensoverdracht" is ingeschakeld. Dit geldt ook voor scenario's die gebruikmaken van SMB 2.1, SMB 3.0 zonder versleuteling en bepaalde versies van de Linux SMB-client.

#### <a name="azure-client-side-encryption"></a>Azure-Client-side '-versleuteling

Een andere optie voor het beveiligen van persoonlijke gegevens, terwijl deze wordt overgebracht tussen een client-toepassing en een Azure Storage is [clientzijde versleuteling](https://docs.microsoft.com/azure/storage/storage-client-side-encryption). De gegevens worden versleuteld voordat ze worden overgedragen naar de Azure-opslag en wanneer u de gegevens uit Azure Storage ophaalt, de gegevens worden ontsleuteld nadat het is ontvangen op de client.

### <a name="azure-site-to-site-vpn"></a>Azure Site-naar-Site-VPN

Een effectieve manier om persoonlijke gegevens onderweg tussen een bedrijfsnetwerk of de gebruiker en de virtuele Azure-netwerk te beveiligen is met een [site-naar-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) of [punt-naar-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) virtuele particuliere netwerk (VPN). Een VPN-verbinding wordt gemaakt van een beveiligde tunnel versleutelde via Internet.

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a>Hoe maak ik een site-naar-site VPN-verbinding

Meerdere gebruikers op het bedrijfsnetwerk verbinding een site-naar-site-VPN-met Azure. Een site-naar-site om verbinding te maken in de Azure portal, het volgende doen:

1. Maak een virtueel netwerk.

2. Geef een DNS-server.

3. Maak het gatewaysubnet.

4. Maak de VPN-gateway. 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. De lokale netwerkgateway maken.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. Configureer het VPN-apparaat.

7. Maak de VPN-verbinding.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. Controleer of de VPN-verbinding.

Zie voor meer gedetailleerde instructies voor het maken van een site-naar-site-verbinding in de Azure portal [verbinding met Site-naar-Site in de Azure Portal maken]. (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a>Hoe maak ik een punt-naar-site VPN-verbinding

Een punt-naar-Site VPN maakt een beveiligde verbinding van een afzonderlijke clientcomputer met een virtueel netwerk. Dit is handig als u verbinding maken met Azure vanaf een externe locatie wilt, zoals vanaf thuis of een hotel of conferentie center. Een punt-naar-site om verbinding te maken in de Azure portal

1. Maak een virtueel netwerk.

2. Voeg een gatewaysubnet.

3. Geef een DNS-server. (optioneel)

4. Maak een virtuele netwerkgateway.

5. Genereren van certificaten.

6. Toevoegen van de client-adresgroep.

7. De openbare certificaatgegevens van het root-certificaat uploaden.

8. Genereren en de configuratie van VPN-clientpakket installeren.

9. Een geëxporteerde certificaat installeren.

10. Verbinding maken met Azure.

11. Controleer de verbinding.

Zie voor meer instructies, [een punt-naar-Site-verbinding met een VNet met behulp van verificatie via certificaat configureren: Azure Portal.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)

### <a name="ssltls"></a>SSL/TLS

Microsoft raadt aan dat u altijd SSL/TLS-protocollen gebruiken voor het uitwisselen van gegevens op verschillende locaties. Organisaties die gebruiken [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) grote gegevenssets verplaatsen via een speciale snelle WAN koppeling ook de gegevens op toepassingsniveau kunt versleutelen met behulp van SSL/TLS- of andere protocollen voor extra beveiliging.

### <a name="encryption-by-default"></a>Codering standaard

Microsoft maakt gebruik van versleuteling om gegevens onderweg tussen Azure-cloudservices en klanten te beschermen. Als u met Azure Storage via de Azure-Portal communiceert, worden alle transacties plaatsvinden via HTTPS.

[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) is het protocol dat Microsoft-datacenters probeert te onderhandelen met clientsystemen die verbinding met Microsoft-cloudservices maken. TLS bevat sterke verificatie, privacy van berichten en integriteit (schakelt u het detecteren van bericht knoeien, onderschept en kunnen worden vervalst), interoperabiliteit, algoritmeflexibiliteit, gebruiksgemak en gebruik.

[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) wordt ook gebruikt zodat elke verbinding tussen clientsystemen klanten en cloudservices van Microsoft gebruiken unieke sleutels. Verbindingen met Microsoft cloud-services ook te profiteren van op basis van RSA 2048-bits versleuteling sleutellengten. De combinatie van TLS, sleutellengtes RSA 2048-bits, en PFS maakt het moeilijker voor iemand om te onderscheppen en toegang tot gegevens die onderweg tussen Microsoft-cloudservices en -klanten.

Gegevens die onderweg is altijd versleuteld in [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview). Naast het versleutelen van gegevens voorafgaand aan het opslaan op permanente media, worden de gegevens tijdens overdracht ook altijd beveiligd met behulp van HTTPS. HTTPS is het enige protocol dat wordt ondersteund voor de Data Lake Store REST-interfaces.

## <a name="summary"></a>Samenvatting

Het bedrijf kan het doel van persoonlijke gegevens en de privacy van dergelijke gegevens beveiligen met HTTPS-verbindingen naar Azure Storage afdwingen, met behulp van handtekeningen voor gedeelde toegang en inschakelen van beveiligde overdracht vereist op de storage-accounts kunt uitvoeren. Ze kunnen ook persoonlijke gegevens beveiligen met behulp van SMB 3.0-verbindingen en clientzijde codering implementeren. Site-naar-site VPN-verbindingen met het bedrijfsnetwerk naar het Azure-netwerk en punt-naar-site VPN-verbindingen van afzonderlijke gebruikers maakt een beveiligde tunnel via welke persoonlijke gegevens kan veilig worden verzonden. Procedures voor versleuteling van Microsoft-standaard wordt de privacy van persoonlijke gegevens verder beschermen.

## <a name="next-steps"></a>Volgende stappen

- [Best Practices voor beveiliging van gegevens van Azure en versleuteling](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [Planning en ontwerp voor VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [Veelgestelde vragen VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [Kopen en configureren van een SSL-certificaat voor uw Azure App Service](https://docs.microsoft.com/azure/app-service/web-sites-purchase-ssl-web-site)

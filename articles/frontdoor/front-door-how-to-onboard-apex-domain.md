---
title: Onboarding een basis- of apex domein naar een bestaande voordeur met behulp van de Azure portal
description: Informatie over hoe moet worden vrijgegeven een basis- of apex domein naar een bestaande voordeur met behulp van de Azure portal.
services: front-door
author: sharad4u
ms.service: front-door
ms.topic: article
ms.date: 5/21/2019
ms.author: sharadag
ms.openlocfilehash: 464c38b0ece274d2ea7df89ab9fd7c0a60dc4b58
ms.sourcegitcommit: 08138eab740c12bf68c787062b101a4333292075
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2019
ms.locfileid: "67333282"
---
# <a name="onboard-a-root-or-apex-domain-on-your-front-door"></a>Onboarding een basis- of het toppunt van domein op de voordeur
Azure voordeur maakt gebruik van CNAME-records voor het valideren van domeineigendom voor het voorbereiden van aangepaste domeinen. Ook voordeur het frontend-IP-adres dat is gekoppeld aan uw profiel voordeur niet beschikbaar en zodat u niet kan uw apex domein toewijzen aan een IP-adres als het doel om onboarding deze aan de voordeur voor Azure is.

Het DNS-protocol wordt voorkomen dat de toewijzing van CNAME-records in de apex van de zone. Bijvoorbeeld, als uw domein is `contoso.com`; u kunt maken van CNAME-records voor `somelabel.contoso.com`; maar u kunt geen CNAME voor maken `contoso.com` zelf. Deze beperking geeft een probleem voor toepassingseigenaren van die toepassingen met load balancing achter Azure voordeur hebben. Omdat het maken van een CNAME-record met behulp van een profiel voordeur worden vereist, is het niet mogelijk om te verwijzen naar de voordeur-profiel in het toppunt van de zone.

Dit probleem is opgelost met aliasrecords in Azure DNS. In tegenstelling tot de CNAME-records, aliasrecords zijn gemaakt in de apex van de zone en toepassingseigenaren kunnen deze gebruiken om hun apexrecord zone verwijzen naar een voordeur-profiel met openbare eindpunten. Toepassingseigenaren verwijzen naar hetzelfde voordeur profiel dat wordt gebruikt voor andere domeinen in hun DNS-zone. Bijvoorbeeld, `contoso.com` en `www.contoso.com` kan verwijzen naar hetzelfde voordeur profiel. 

Uw domein het toppunt van of het basiscertificaat in feite toewijzen aan uw profiel voordeur vereist CNAME plat maken of DNS-chasing, dit is een mechanisme waarbij in de DNS-Server provider recursief oplossing voor de CNAME-vermelding totdat deze komt binnen via een IP-adres. Deze functionaliteit wordt ondersteund door Azure DNS voor de voordeur eindpunten. 

> [!NOTE]
> Er zijn andere DNS-providers ook die ondersteuning bieden voor CNAME plat maken of DNS chasing, echter Azure voordeur raadt het gebruik van Azure DNS zijn klanten voor het hosten van hun domeinen.

U kunt het gebruiken van de Azure portal voor onboarding een apex-domein op uw voordeur en HTTPS hebt ingeschakeld voor het door deze te koppelen met een certificaat voor SSL-beëindiging. Het toppunt van domeinen worden ook wel aangeduid als root of zonder voorvoegsel zijn domeinen.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Maak een alias-record die naar uw profiel voordeur verwijst
> * Het hoofddomein toevoegen aan de voordeur
> * HTTPS voor het hoofddomein instellen

> [!NOTE]
> Deze zelfstudie vereist dat u al een voordeur profiel hebt gemaakt. Raadpleeg andere zelfstudies, zoals [Quick Start: Maken van een voordeur](./quickstart-create-front-door.md) of [een voordeur maken via HTTP naar HTTPS-omleiding](./front-door-how-to-redirect-https.md) aan de slag.

## <a name="create-an-alias-record-for-zone-apex"></a>Een alias-record voor het toppunt van zone maken

1. Open **Azure DNS** configuratie voor het domein waarvoor onboarding moet worden.
2. Maak of bewerk de record voor het toppunt van de zone.
3. Selecteer de record **type** als _A_ opnemen en selecteer vervolgens _Ja_ voor **Alias-Recordset**. **Aliastype** moet worden ingesteld op _Azure-resource_.
4. Kies het Azure-abonnement waar uw profiel voordeur wordt gehost en selecteer vervolgens de voordeur-resource in de **Azure-resource** vervolgkeuzelijst.
5. Klik op **OK** uw wijzigingen wilt indienen.

    ![Voor het toppunt van zone-alias-record](./media/front-door-apex-domain/front-door-apex-alias-record.png)

6. De bovenstaande stap maakt u een zone apex-record die verwijst naar de voordeur-bron en ook een toewijzing CNAME-record afdverify (voorbeeld: `afdverify.contosonews.com`) naar `afdverify.<name>.azurefd.net` die worden gebruikt voor de voorbereiding op het domein in uw profiel voordeur.

## <a name="onboard-the-custom-domain-on-your-front-door"></a>Onboarding het aangepaste domein in de voordeur

1. Op het tabblad van de voordeur-ontwerper, klikt u op '+' pictogram op de front-hosts deel een nieuw aangepast domein toevoegt.
2. Voer de basis- of apex domain-name in het naamveld aangepaste host voorbeeld `contosonews.com`.
3. Als de CNAME-toewijzing van het domein aan de deur wordt gevalideerd, klikt u op **toevoegen** om toe te voegen van het aangepaste domein.
4. Klik op **opslaan** de wijzigingen verzendt.

![Menu voor aangepaste domeinen](./media/front-door-apex-domain/front-door-onboard-apex-domain.png)

## <a name="enable-https-on-your-custom-domain"></a>HTTPS voor uw aangepaste domein inschakelen

1. Klik op het aangepaste domein dat is toegevoegd en onder de sectie **HTTPS voor aangepaste domeinen**, wijzigt de status van de **ingeschakeld**.
2. Selecteer de **management certificaattype** naar _'Mijn eigen certificaat gebruiken'_ .

> [!WARNING]
> Front klep van beheerde certificaatbeheertype is momenteel niet ondersteund voor apex of het basiscertificaat domeinen. De enige optie die beschikbaar zijn voor het inschakelen van HTTPS op een domein apex of het basiscertificaat voor de voordeur maakt gebruik van uw eigen aangepaste SSL-certificaat die wordt gehost op Azure Key Vault.

3. Zorg ervoor dat u ingesteld de juiste machtigingen voor de voordeur hebt voor toegang tot uw key Vault, zoals vermeld in de gebruikersinterface, voordat u doorgaat met de volgende stap.
4. Kies een **Key Vault-account** van uw huidige abonnement en selecteer vervolgens de juiste **geheim** en **geheime versie** om toe te wijzen aan het juiste certificaat.
5. Klik op **Update** opslaan van de selectie en klik vervolgens op **opslaan**.
6. Klik op **vernieuwen** na een paar minuten en klik vervolgens op het aangepaste domein opnieuw om te zien van de voortgang van de inrichting van certificaten. 

> [!WARNING]
> Zorg ervoor dat u hebt gemaakt van de juiste routeringsregels voor uw domein het toppunt van of het domein toegevoegd aan bestaande regels voor doorsturen.

## <a name="next-steps"></a>Volgende stappen

- Lees hoe u [een Front Door maakt](quickstart-create-front-door.md).
- Lees [hoe Front Door werkt](front-door-routing-architecture.md).
---
title: Wat is Azure Sleutelkluis? | Microsoft Docs
description: Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. Klanten kunnen met Azure Key Vault sleutels en geheimen versleutelen (zoals verificatiesleutels, opslagaccountsleutels, gegevensversleutelingssleutels, PFX-bestanden en wachtwoorden) door gebruik te maken van sleutels die worden beveiligd met HSM's.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/02/2018
ms.author: barclayn
ms.openlocfilehash: 08331a399044eba17060d15f24af1863df38caf5
ms.sourcegitcommit: fc5555a0250e3ef4914b077e017d30185b4a27e6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39480250"
---
# <a name="what-is-azure-key-vault"></a>Wat is Azure Sleutelkluis?

Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. U kunt met Key Vault sleutels en geheimen versleutelen (zoals verificatiesleutels, opslagaccountsleutels, gegevensversleutelingssleutels, PFX-bestanden en wachtwoorden) door gebruik te maken van sleutels die worden beveiligd met HSM's (Hardware Security Modules). Voor extra zekerheid kunt u de sleutels importeren of genereren in HSM's. Als u hiervoor kiest, verwerkt Microsoft uw sleutels in FIPS 140-2 Level 2-gevalideerde HSM's (hardware en firmware).  

Key Vault stroomlijnt het beheerproces voor sleutels en zorgt dat u de controle houdt over de sleutels waarmee uw gegevens toegankelijk zijn en worden versleuteld. Ontwikkelaars kunnen binnen enkele minuten sleutels voor ontwikkel- en testdoeleinden maken en deze vervolgens probleemloos migreren naar productiesleutels. Beveiligingsadministrator kunnen wanneer dit nodig is machtigingen aan sleutels verlenen (en intrekken).

## <a name="basic-concepts"></a>Basisbegrippen

Azure Key Vault is een hulpprogramma voor het veilig opslaan en openen van geheimen. Een geheim is alles waartoe u de toegang streng wilt beheren, zoals API-sleutels, wachtwoorden of certificaten.
Hieronder vindt u enkele belangrijke termen:
- **Tenant**: een tenant is de organisatie die een specifiek exemplaar van Microsoft-cloudservices in eigendom heeft en beheert. Deze wordt meestal op een exacte manier gebruikt om te verwijzen naar de set met Azure- en Office 365-services voor een organisatie
- **Kluiseigenaar**: kan een sleutelkluis maken en heeft er volledige toegang toe en controle over. De eigenaar van de kluis kan ook controles instellen om vast te leggen wie toegang heeft gehad tot geheimen en sleutels. Beheerders kunnen de levenscyclus van sleutels beheren. Ze kunnen een nieuwe versie van de sleutel, back-up, enzovoort implementeren.
- **Resource**: een beheerbaar item dat beschikbaar is via Azure. Sommige algemene resources zijn een virtuele machine, opslagaccount, webtoepassing en virtueel netwerk, maar er zijn er veel meer.
- **Resourcegroep**: een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing bevatten of enkel de resources die u als groep wilt beheren. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is. Zie Resourcegroepen.
- **Kluisconsument**: kan acties uitvoeren op de elementen in de sleutelkluis wanneer de eigenaar van de kluis hem/haar toegang verleent, die afhankelijk is van de verleende machtigingen.
- **[Azure Active Directory](../active-directory/active-directory-whatis.md)** is de Azure AD-service voor een bepaalde tenant. Elke adreslijst heeft een of meer domeinen. Aan een directory kunnen vele abonnementen zijn gekoppeld, maar slechts één tenant. 
- **Azure-tenant-ID**: dit is een unieke manier om een Azure Active Directory in een Azure-abonnement te identificeren. 
- **Managed Service Identity**: Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd bij Key Vault om ze op te kunnen halen. Managed Service Identity (MSI) levert Azure-services met een automatisch beheerde identiteit in Azure Active Directory (Azure AD), waarmee dit probleem eenvoudiger kan worden opgelost. U kunt deze identiteit gebruiken voor verificatie bij Key Vault bij alle services die ondersteuning bieden voor Microsoft Azure Active Directory-verificatie, zonder dat u referenties in uw code hoeft te hebben. Meer informatie over MSI leest u [hier](../active-directory/managed-service-identity/overview.md)

    ![MSI graphic](./media/key-vault-whatis/msi.png)

## <a name="key-vault-roles"></a>Rollen van Key Vault

Gebruik de volgende tabel om beter te begrijpen hoe Key Vault u kan helpen om aan de behoeften van ontwikkelaars en beveiligingsadministrators te voldoen.

| Rol | Probleemformulering | Opgelost door Azure Key Vault |
| --- | --- | --- |
| Ontwikkelaar voor een Azure-toepassing |"Ik wil een toepassing voor Azure schrijven die voor de ondertekening en versleuteling gebruikmaakt van sleutels. Dit moeten echter wel externe sleutels zijn, zodat de oplossing geschikt is voor een toepassing die geografisch wordt gedistribueerd. <br/><br/>Ik wil ook dat deze sleutels en geheimen worden beveiligd, zonder dat ik de code zelf hoef te schrijven. Ik wil ook dat deze sleutels en geheimen eenvoudig zijn te gebruiken vanuit mijn toepassingen, met optimale prestaties." |√ De sleutels worden opgeslagen in een kluis en wanneer dit nodig is, aangeroepen via een URI.<br/><br/> √ De sleutels worden beveiligd door Azure. Hiervoor wordt gebruikgemaakt van algoritmen, sleutellengten en HSM's die voldoen aan de industriestandaard).<br/><br/> √ Sleutels worden verwerkt in HSM's die zich in dezelfde Azure-datacenters bevinden als de toepassingen. Dit biedt betere betrouwbaarheid en verminderde latentie dan wanneer de sleutels zich op een andere locatie bevinden, zoals on-premises. |
| SaaS-ontwikkelaar (Software as a Service) |"Ik wil niet de verantwoordelijk of potentiële aansprakelijkheid voor de tenantsleutels en -geheimen van mijn klant op mij nemen. <br/><br/>Ik wil dat klanten hun sleutels zelf in eigendom hebben en beheren, zodat ik mij kan concentreren op hetgeen waar ik het beste in ben, namelijk het leveren van de belangrijkste softwarefuncties." |√ Klanten kunnen hun eigen sleutels in Azure importeren en beheren. Wanneer een SaaS-toepassing cryptografiebewerkingen moet uitvoeren met de sleutels van de klant, voert Key Vault deze bewerkingen namens de toepassing uit. De toepassing krijgt de sleutels van de klant niet te zien. |
| Chief Security Officer (CSO) |"Ik wil weten of onze toepassingen voldoen aan de FIPS 140-2 Level 2 HSM's voor beveiligd sleutelbeheer. <br/><br/>Ik wil ervoor zorgen dat mijn organisatie de controle heeft over de levenscyclus van de sleutel en het sleutelgebruik kan controleren. <br/><br/>En hoewel we meerdere Azure-services en -resources gebruiken, wil ik de sleutels kunnen beheren vanaf één locatie in Azure." |√ HSM's zijn FIPS 140-2 Level 2-gevalideerde modules.<br/><br/>√ Key Vault is zodanig ontworpen dat Microsoft uw sleutels niet kan zien of extraheren.<br/><br/>√ Het sleutelgebruik wordt vrijwel in realtime geregistreerd in een logboek.<br/><br/>√ De kluis biedt één interface, ongeacht het aantal kluizen dat u in Azure hebt, welke regio's ze ondersteunen en door welke toepassingen ze worden gebruikt. |

Iedereen met een Azure-abonnement kan sleutelkluizen maken en gebruiken. Hoewel Key Vault voordelen biedt voor ontwikkelaars en beveiligingsadministrators, kan de service worden geïmplementeerd en beheerd door een beheerder die ook andere Azure-services voor een organisatie beheert. De beheerder kan zich bijvoorbeeld aanmelden met een Azure-abonnement, vervolgens een kluis voor de organisatie maken waarin sleutels worden opgeslagen en zich ontfermen over operationele taken, zoals:

* Een sleutel of geheim maken of importeren.
* Een sleutel of geheim intrekken of verwijderen.
* Gebruikers of toepassingen machtigingen verlenen voor toegang tot de sleutelkluis, zodat ze de sleutels en geheimen in de kluis kunnen beheren of gebruiken.
* Sleutelgebruik configureren (bijvoorbeeld ondertekenen of versleutelen).
* Sleutelgebruik bewaken.

De beheerder kan de ontwikkelaars vervolgens voorzien van URI's die kunnen worden aangeroepen vanuit hun toepassingen. Daarnaast kan de beheerder de beveiligingsadministrator logboekregistratiegegevens over het gebruik van de sleutel verstrekken. 

   ![Overzicht van Azure Key Vault][1]

Ontwikkelaars kunnen de sleutels ook rechtstreeks beheren door gebruik te maken van API's. Zie [Ontwikkelaarshandleiding voor Key Vault](key-vault-developers-guide.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Zie [Aan de slag met Azure Key Vault](key-vault-get-started.md) voor een inleidende zelfstudie voor beheerders.

Zie [Logboekregistratie van Azure Key Vault](key-vault-logging.md) voor meer informatie over de gebruiksregistratie voor Key Vault.

Zie [Over sleutels, geheimen en certificaten](https://msdn.microsoft.com/library/azure/dn903623\(v=azure.1\).aspx) voor meer informatie over het gebruik van sleutels en geheimen met Azure Key Vault.

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure Sleutelkluis is beschikbaar in de meeste regio's. Zie de pagina [Prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie.

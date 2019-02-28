---
title: Wat is Azure Sleutelkluis? | Microsoft Docs
description: Informatie over hoe Azure Key Vault beschermt cryptografische sleutels en geheimen die cloud-toepassingen en services te gebruiken.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/18/2019
ms.author: barclayn
ms.openlocfilehash: 48ac0c3efe74723099e87a77871aa1a78834efbd
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2019
ms.locfileid: "56958529"
---
# <a name="what-is-azure-key-vault"></a>Wat is Azure Sleutelkluis?

Cloud-toepassingen en services met cryptografische sleutels en geheimen om te zorgen dat gegevens beveiligen. Azure Key Vault beschermt deze sleutels en geheimen. Wanneer u Key Vault gebruikt, kunt u verificatiesleutels, opslagaccountsleutels, gegevensversleutelingssleutels, pfx-bestanden en wachtwoorden versleutelen met sleutels die zijn beveiligd door hardware security modules (HSM's).

Key Vault helpt bij het oplossen van de volgende problemen:

- **Geheimenbeheer**: Veilig opslaan en toegang tot tokens, wachtwoorden, certificaten, API-sleutels en andere geheimen nauw beheren.
- **Sleutelbeheer**: Maken en beheren van de versleutelingssleutels die uw gegevens te versleutelen. 
- **Certificaatbeheer**: Inrichten, beheren en implementeren van openbare en persoonlijke Secure Sockets Layer/Transport Layer Security (SSL/TLS)-certificaten voor gebruik met Azure en uw interne verbonden bronnen. 
- **Geheimen die worden ondersteund door een HSM's Store**: Software of FIPS 140-2 niveau 2 HSM's gevalideerde ter bescherming van geheimen en sleutels gebruiken.

## <a name="basic-concepts"></a>Basisbegrippen

Azure Key Vault is een hulpprogramma voor het veilig opslaan en openen van geheimen. Een geheim is alles waartoe u de toegang streng wilt beheren, zoals API-sleutels, wachtwoorden of certificaten. Een kluis is een logische groep van geheimen.

Hier volgen andere belangrijke termen:

- **Tenant**: Een tenant is de organisatie die eigenaar is van en beheert een specifiek exemplaar van Microsoft-cloudservices. Dit wordt meestal gebruikt om te verwijzen naar de set met Azure en Office 365-services voor een organisatie.

- **De eigenaar van de kluis**: De eigenaar van een kluis kan maken van een key vault en krijg volledige toegang en controle over het. De eigenaar van de kluis kan ook controles instellen om vast te leggen wie toegang heeft tot geheimen en sleutels. Beheerders kunnen de levenscyclus van sleutels beheren. Ze kunnen een nieuwe versie van de sleutel instellen, een back-up maken en gerelateerde taken uitvoeren.

- **Kluis consument**: Een kluis consument kan acties uitvoeren op de elementen in de key vault wanneer de eigenaar van de kluis toegang tot de consument verleent. De beschikbare acties zijn afhankelijk van de verleende machtigingen.

- **Resource**: Een resource is een beheerbaar item dat is beschikbaar via Azure. Algemene voorbeelden zijn virtuele machine, opslagaccount, web-app, database en virtueel netwerk. Er zijn nog veel meer.

- **Resourcegroep**: Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing bevatten of enkel de resources die u als groep wilt beheren. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is.

- **Service-principal**: Een Azure service-principal is een beveiligings-id die door de gebruiker gemaakte apps, services en automatiseringsprogramma's gebruiken voor toegang tot specifieke Azure-resources. Vergelijken met een 'gebruikers-id' (gebruikersnaam en wachtwoord of certificaat) die een specifieke rol heeft en over nauwkeurig omschreven machtigingen beschikt. Anders dan een algemene gebruikers-id, hoeft een service-principal slechts enkele specifieke handelingen uit te kunnen voeren. Dit verbetert de beveiliging als u deze alleen het niveau minimale machtigingen die nodig is om uit te voeren van de beheertaken worden verleend.

- [Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md): Azure AD is de Active Directory-service voor een tenant. Elke adreslijst heeft een of meer domeinen. Aan een directory kunnen vele abonnementen zijn gekoppeld, maar slechts één tenant.

- **Azure-tenant-ID**: Een tenant-ID is een unieke manier om een Azure AD-exemplaar binnen een Azure-abonnement te identificeren.

- **Beheerde identiteiten**: Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Key Vault om ze op te halen. Met behulp van een beheerde identiteit, maakt het oplossen van dit probleem eenvoudiger door middel van Azure-services een automatisch beheerde identiteit in Azure AD. U kunt deze identiteit gebruiken voor verificatie bij Key Vault bij alle services die ondersteuning bieden voor Microsoft Azure Active Directory-verificatie, zonder dat u referenties in uw code hoeft te hebben. Zie voor meer informatie de volgende afbeelding en de [overzicht van beheerde identiteiten voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md).

    ![Diagram van hoe beheerde identiteiten voor Azure-resources](./media/key-vault-whatis/msi.png)

## <a name="authentication"></a>Authentication
Als u wilt doen bewerkingen met Key Vault, moet u eerst om deze te verifiëren. Er zijn drie manieren om te verifiëren naar Key Vault:

- [Identiteiten voor een Azure-resources beheerd](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview): Wanneer u een app op een virtuele machine in Azure implementeert, kunt u een identiteit toewijzen aan uw virtuele machine die toegang tot de Key Vault heeft. U kunt ook toewijzen identiteiten met [andere Azure-resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Het voordeel van deze benadering is dat de app of service de draaihoek van het eerste geheim is niet beheren. Azure draait automatisch de identiteit. Wordt deze methode als een best practice aangeraden. 
- **Service-principal en certificaat**: U kunt een service-principal en een bijbehorende certificaat dat toegang tot de Key Vault heeft gebruiken. Deze methode wordt niet aanbevolen omdat de toepassingseigenaar van de of de ontwikkelaar moet het certificaat draaien.
- **Service-principal en -geheim**: Hoewel u een service-principal en een geheim gebruiken kunt om te verifiëren naar Key Vault, aanbevolen niet het. Het is moeilijk te roteer automatisch het bootstrap geheim dat wordt gebruikt voor verificatie aan Key Vault.


## <a name="key-vault-roles"></a>Rollen van Key Vault

Gebruik de volgende tabel om beter te begrijpen hoe Key Vault u kan helpen om aan de behoeften van ontwikkelaars en beveiligingsadministrators te voldoen.

| Rol | Probleemformulering | Opgelost door Azure Key Vault |
| --- | --- | --- |
| Ontwikkelaar voor een Azure-toepassing |"Ik wil een toepassing voor Azure schrijven die gebruikmaakt van sleutels voor ondertekening en versleuteling. Maar ik wil deze externe sleutels zijn van mijn toepassing zodat de oplossing geschikt is voor een toepassing die geografisch wordt gedistribueerd. <br/><br/>Ik wil dat deze sleutels en geheimen worden beveiligd, zonder dat ik de code zelf hoef te schrijven. Ik wil ook dat deze sleutels en geheimen eenvoudig zijn te gebruiken vanuit mijn toepassingen, met optimale prestaties." |√ De sleutels worden opgeslagen in een kluis en wanneer dit nodig is, aangeroepen via een URI.<br/><br/> √ De sleutels worden beveiligd door Azure. Hiervoor wordt gebruikgemaakt van algoritmen, sleutellengten en HSM's die voldoen aan de industriestandaard.<br/><br/> √ Sleutels worden verwerkt in HSM's die zich in dezelfde Azure-datacenters bevinden als de toepassingen. Deze methode biedt betere betrouwbaarheid en minder latentie dan wanneer de sleutels zich op een afzonderlijke locatie bevinden, zoals on-premises. |
| SaaS-ontwikkelaar (Software as a Service) |"Ik wil niet de verantwoordelijk of potentiële aansprakelijkheid voor de tenantsleutels en -geheimen van mijn klant op mij nemen. <br/><br/>Ik wil dat klanten hun sleutels zelf in eigendom hebben en beheren, zodat ik mij kan concentreren op de zaken waar ik het beste in ben, namelijk het leveren van de kernsoftwarefuncties." |√ Klanten kunnen hun eigen sleutels in Azure importeren en beheren. Wanneer een SaaS-toepassing nodig heeft om uit te voeren cryptografische bewerkingen met behulp van klanten-sleutels, wordt de Key Vault deze bewerkingen namens de toepassing. De toepassing krijgt de sleutels van de klant niet te zien. |
| Chief Security Officer (CSO) |"Ik wil weten of onze toepassingen voldoen aan de FIPS 140-2 Level 2 HSM's voor beveiligd sleutelbeheer. <br/><br/>Ik wil ervoor zorgen dat mijn organisatie de controle heeft over de levenscyclus van de sleutel en het sleutelgebruik kan bewaken. <br/><br/>En hoewel we meerdere Azure-services en -resources gebruiken, wil ik de sleutels kunnen beheren vanaf één locatie in Azure." |√ HSM's zijn FIPS 140-2 Level 2-gevalideerde modules.<br/><br/>√ Key Vault is zodanig ontworpen dat Microsoft uw sleutels niet kan zien of extraheren.<br/><br/>√ Sleutelgebruik wordt vrijwel in realtime vastgelegd.<br/><br/>√ De kluis biedt één interface, ongeacht het aantal kluizen dat u in Azure hebt, welke regio's ze ondersteunen en door welke toepassingen ze worden gebruikt. |

Iedereen met een Azure-abonnement kan sleutelkluizen maken en gebruiken. Hoewel Key Vault voordelen biedt voor ontwikkelaars en beveiligingsadministrators, kan worden geïmplementeerd en beheerd door de beheerder van een organisatie van andere Azure-services. Deze beheerder kan zich aanmelden met een Azure-abonnement maakt bijvoorbeeld een kluis voor de organisatie waarin sleutels opslaan en vervolgens worden die verantwoordelijk is voor operationele taken zoals deze:

- Een sleutel of geheim maken of importeren.
- Een sleutel of geheim intrekken of verwijderen.
- Gebruikers of toepassingen machtigingen verlenen voor toegang tot de sleutelkluis, zodat ze de sleutels en geheimen in de kluis kunnen beheren of gebruiken.
- Sleutelgebruik configureren (bijvoorbeeld ondertekenen of versleutelen).
- Sleutelgebruik bewaken.

Deze beheerder geeft ontwikkelaars URI's om aan te roepen via hun toepassingen. Deze beheerder geeft ook logboekinformatie voor gebruik van de sleutel tot de beveiligingsbeheerder. 

![Overzicht van de werking van Azure Key Vault][1]

Ontwikkelaars kunnen de sleutels ook rechtstreeks beheren door gebruik te maken van API's. Zie [Ontwikkelaarshandleiding voor Key Vault](key-vault-developers-guide.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beveiligen van uw kluis](key-vault-secure-your-key-vault.md).

<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png
Azure Sleutelkluis is beschikbaar in de meeste regio's. Zie de pagina [Prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie.

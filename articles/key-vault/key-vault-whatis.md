---
title: Wat is Azure Sleutelkluis? -Azure Key Vault | Microsoft Docs
description: Azure Key Vault Beveilig cryptografische sleutels en geheimen die worden gebruikt door cloudtoepassingen en -services. Klanten kunnen verificatiesleutels, opslagaccountsleutels, gegevensversleutelingssleutels, coderen. PFX-bestanden en wachtwoorden met sleutels die zijn beveiligd door hardware security modules (HSM's).
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e759df6f-0638-43b1-98ed-30b3913f9b82
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: barclayn
ms.openlocfilehash: 889f12b935eaad72eb7637524ab578f28fbc412f
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54198487"
---
# <a name="what-is-azure-key-vault"></a>Wat is Azure Sleutelkluis?

Met Azure Key Vault kunt u de volgende problemen oplossen:

- **Geheimen Management** -Azure Key Vault kan worden gebruikt voor het veilig opslaan en toegang tot tokens, wachtwoorden, certificaten, API-sleutels en andere geheimen nauw beheren.
- **Sleutelbeheer** - U kunt Azure Key Vault ook gebruiken als een oplossing voor sleutelbeheer. Met Azure Key Vault kunt u eenvoudig de versleutelingssleutels maken en beheren waarmee uw gegevens worden versleuteld. 
- **Certificaatbeheer** - Azure Key Vault is ook een service waarmee u eenvoudig openbare en persoonlijke SSL/TLS-certificaten (Secure Sockets Layer/Transport Layer Security) kunt inrichten, beheren en implementeren voor gebruik met Azure en uw interne verbonden bronnen. 
- **Geheimen die worden ondersteund door Hardware Security Modules Store** -geheimen en sleutels kunnen worden beveiligd door software of FIPS 140-2 Level 2 HSM's valideert.

## <a name="basic-concepts"></a>Basisbegrippen

Azure Key Vault is een hulpprogramma voor het veilig opslaan en openen van geheimen. Een geheim is alles waartoe u de toegang streng wilt beheren, zoals API-sleutels, wachtwoorden of certificaten. Een **kluis** is logische groep van geheimen. Nu u alle bewerkingen met Key Vault moet u eerst om deze te verifiëren. 

Er zijn fundamenteel drie manieren om te verifiëren naar Key Vault:

1. **Met behulp van [beheerde identiteiten voor een Azure-resources](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)**  (**aanbevolen en beste praktijken**): Wanneer u een App op een virtuele Machine in Azure implementeert, kunt u een identiteit toewijzen aan uw virtuele Machine die toegang tot de Key Vault heeft. U kunt identiteiten ook toewijzen aan andere azure-resources die worden vermeld [hier](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview). Het voordeel met deze methode is de app / service wordt niet beheerd door de draaihoek van het eerste geheim. Azure draait automatisch de identiteit. 
2. **Met behulp van Service-Principal en certificaat:** De tweede optie is het gebruik van een Service-Principal en een bijbehorende certificaat dat toegang tot de Key Vault heeft. De plicht draaien van het certificaat is op de toepassingseigenaar van de of de ontwikkelaar en kan daarom dit wordt niet aanbevolen.
3. **Met behulp van Service-Principal en -geheim:** De derde optie (niet aanbevolen optie) is een Service-Principal en een geheim gebruiken om te verifiëren naar Key Vault.

> [!NOTE]
> De bovenstaande 3e optie mag niet worden gebruikt als het is moeilijk te automatisch draaien het bootstrap geheim gebruikt voor authenticatie in Key Vault.

Hieronder vindt u een paar belangrijke termen:

- **Tenant**: Een tenant is de organisatie die eigenaar is van en beheert een specifiek exemplaar van Microsoft-cloudservices. Deze wordt meestal op een exacte manier gebruikt om te verwijzen naar de set met Azure- en Office 365-services voor een organisatie.
- **De eigenaar van de kluis**: De eigenaar van een kluis kan maken van een key vault en krijg volledige toegang en controle over het. De eigenaar van de kluis kan ook controles instellen om vast te leggen wie toegang heeft tot geheimen en sleutels. Beheerders kunnen de levenscyclus van sleutels beheren. Ze kunnen een nieuwe versie van de sleutel instellen, een back-up maken en gerelateerde taken uitvoeren.
- **Kluis consument**: Een kluis consument kan acties uitvoeren op de elementen in de key vault wanneer de eigenaar van de kluis toegang tot de consument verleent. De beschikbare acties zijn afhankelijk van de verleende machtigingen.
- **Resource**: Een resource is een beheerbaar item dat is beschikbaar via Azure. Sommige algemene resources zijn een virtuele machine, opslagaccount, webtoepassing en virtueel netwerk, maar er zijn er veel meer.
- **Resourcegroep**: Een resourcegroep is een container met gerelateerde resources voor een Azure-oplossing. De resourcegroep kan alle resources voor de oplossing bevatten of enkel de resources die u als groep wilt beheren. U bepaalt hoe resources worden toegewezen aan resourcegroepen op basis van wat voor uw organisatie het meest zinvol is.
- **Service-Principal** -een Azure service-principal is een beveiligings-id die is gebruikt door gebruiker gemaakte apps, services en automatiseringsprogramma's voor toegang tot specifieke Azure-resources. U kunt een service-principal vergelijken met een gebruikers-id (gebruikersnaam en wachtwoord of certificaat) die een specifieke rol heeft en over nauwkeurig omschreven machtigingen beschikt. Anders dan een algemene gebruikers-id, hoeft een service-principal slechts enkele specifieke handelingen uit te kunnen voeren. De beveiliging verbetert erdoor als u er net voldoende machtigingen aan verleent om de beheertaken te kunnen uitvoeren.
- **[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md)**: Azure AD is de Active Directory-service voor een tenant. Elke adreslijst heeft een of meer domeinen. Aan een directory kunnen vele abonnementen zijn gekoppeld, maar slechts één tenant. 
- **Azure-tenant-ID**: Een tenant-ID is een unieke manier om een Azure AD-exemplaar binnen een Azure-abonnement te identificeren.
- **Identiteiten voor een Azure-resources beheerd**: Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Key Vault om ze op te halen. Met behulp van een beheerde identiteit, maakt het oplossen van dit probleem eenvoudiger door middel van Azure-services een automatisch beheerde identiteit in Azure AD. U kunt deze identiteit gebruiken voor verificatie bij Key Vault bij alle services die ondersteuning bieden voor Microsoft Azure Active Directory-verificatie, zonder dat u referenties in uw code hoeft te hebben. Voor meer informatie, Zie de onderstaande afbeelding en de [beheerde identiteiten voor een overzicht van Azure-resources](../active-directory/managed-identities-azure-resources/overview.md).

    ![Diagram van hoe u beheerde identiteiten voor Azure-resources](./media/key-vault-whatis/msi.png)

## <a name="key-vault-roles"></a>Rollen van Key Vault

Gebruik de volgende tabel om beter te begrijpen hoe Key Vault u kan helpen om aan de behoeften van ontwikkelaars en beveiligingsadministrators te voldoen.

| Rol | Probleemformulering | Opgelost door Azure Key Vault |
| --- | --- | --- |
| Ontwikkelaar voor een Azure-toepassing |"Ik wil een toepassing voor Azure schrijven die voor de ondertekening en versleuteling gebruikmaakt van sleutels. Dit moeten echter wel externe sleutels zijn, zodat de oplossing geschikt is voor een toepassing die geografisch wordt gedistribueerd. <br/><br/>Ik wil dat deze sleutels en geheimen worden beveiligd, zonder dat ik de code zelf hoef te schrijven. Ik wil ook dat deze sleutels en geheimen eenvoudig zijn te gebruiken vanuit mijn toepassingen, met optimale prestaties." |√ De sleutels worden opgeslagen in een kluis en wanneer dit nodig is, aangeroepen via een URI.<br/><br/> √ De sleutels worden beveiligd door Azure. Hiervoor wordt gebruikgemaakt van algoritmen, sleutellengten en HSM's die voldoen aan de industriestandaard.<br/><br/> √ Sleutels worden verwerkt in HSM's die zich in dezelfde Azure-datacenters bevinden als de toepassingen. Deze methode biedt betere betrouwbaarheid en minder latentie dan wanneer de sleutels zich op een afzonderlijke locatie bevinden, zoals on-premises. |
| SaaS-ontwikkelaar (Software as a Service) |"Ik wil niet de verantwoordelijk of potentiële aansprakelijkheid voor de tenantsleutels en -geheimen van mijn klant op mij nemen. <br/><br/>Ik wil dat klanten hun sleutels zelf in eigendom hebben en beheren, zodat ik mij kan concentreren op de zaken waar ik het beste in ben, namelijk het leveren van de kernsoftwarefuncties." |√ Klanten kunnen hun eigen sleutels in Azure importeren en beheren. Wanneer een SaaS-toepassing cryptografiebewerkingen moet uitvoeren met de sleutels van de klant, voert Key Vault deze bewerkingen namens de toepassing uit. De toepassing krijgt de sleutels van de klant niet te zien. |
| Chief Security Officer (CSO) |"Ik wil weten of onze toepassingen voldoen aan de FIPS 140-2 Level 2 HSM's voor beveiligd sleutelbeheer. <br/><br/>Ik wil ervoor zorgen dat mijn organisatie de controle heeft over de levenscyclus van de sleutel en het sleutelgebruik kan bewaken. <br/><br/>En hoewel we meerdere Azure-services en -resources gebruiken, wil ik de sleutels kunnen beheren vanaf één locatie in Azure." |√ HSM's zijn FIPS 140-2 Level 2-gevalideerde modules.<br/><br/>√ Key Vault is zodanig ontworpen dat Microsoft uw sleutels niet kan zien of extraheren.<br/><br/>√ Sleutelgebruik wordt vrijwel in realtime vastgelegd.<br/><br/>√ De kluis biedt één interface, ongeacht het aantal kluizen dat u in Azure hebt, welke regio's ze ondersteunen en door welke toepassingen ze worden gebruikt. |

Iedereen met een Azure-abonnement kan sleutelkluizen maken en gebruiken. Hoewel Key Vault voordelen biedt voor ontwikkelaars en beveiligingsbeheerders, kan de service worden geïmplementeerd en beheerd door een beheerder die ook andere Azure-services voor een organisatie beheert. De beheerder kan zich bijvoorbeeld aanmelden met een Azure-abonnement, een kluis voor de organisatie maken waarin sleutels worden opgeslagen en verantwoordelijk zijn voor operationele taken, zoals:

- Een sleutel of geheim maken of importeren.
- Een sleutel of geheim intrekken of verwijderen.
- Gebruikers of toepassingen machtigingen verlenen voor toegang tot de sleutelkluis, zodat ze de sleutels en geheimen in de kluis kunnen beheren of gebruiken.
- Sleutelgebruik configureren (bijvoorbeeld ondertekenen of versleutelen).
- Sleutelgebruik bewaken.

De beheerder kan de ontwikkelaars vervolgens voorzien van URI's die kunnen worden aangeroepen vanuit hun toepassingen. Daarnaast kan de beheerder de beveiligingsadministrator logboekregistratiegegevens over het gebruik van de sleutel verstrekken. 

! [Overzicht van de werking van Azure Key Vault] [1]

Ontwikkelaars kunnen de sleutels ook rechtstreeks beheren door gebruik te maken van API's. Zie [Ontwikkelaarshandleiding voor Key Vault](key-vault-developers-guide.md) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beveiligen van uw kluis](key-vault-secure-your-key-vault.md) 
 <!--Image references--> [1]:./media/key-vault-whatis/AzureKeyVault_overview.png Azure Key Vault is beschikbaar in de meeste regio's. Zie de pagina [Prijzen van Key Vault](https://azure.microsoft.com/pricing/details/key-vault/) voor meer informatie.

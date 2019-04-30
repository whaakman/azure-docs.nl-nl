---
title: OpenShift Container Platform zelfbeheerde Marketplace-aanbieding in Azure implementeren | Microsoft Docs
description: OpenShift Container Platform zelfbeheerde Marketplace-aanbieding in Azure implementeren.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/18/2019
ms.author: haroldw
ms.openlocfilehash: 1228c770799de37c85b8a48b1dc923ac8294eeca
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60773464"
---
# <a name="configure-prerequisites"></a>Vereisten configureren

Voordat u de Marketplace-aanbieding voor een zelfbeheerde OpenShift Container Platform-cluster in Azure implementeert, moeten enkele vereisten worden geconfigureerd.  Lees de [vereisten van OpenShift](https://docs.microsoft.com/azure/virtual-machines/linux/openshift-prerequisites) artikel voor instructies over het maken van een ssh-sleutel (zonder een wachtwoordzin), Azure key vault, key vault-geheim en een service-principal.

 
## <a name="deploy-using-the-marketplace-offer"></a>Implementeren met behulp van de Marketplace-aanbieding

De eenvoudigste manier om een zelfbeheerde OpenShift Container Platform-cluster implementeren in Azure is met de [Azure Marketplace-aanbieding](https://azuremarketplace.microsoft.com/marketplace/apps/redhat.openshift-container-platform?tab=Overview).

Deze optie is de eenvoudigste, maar deze ook beperkte aanpassingsmogelijkheden. De Marketplace-aanbieding voor OpenShift Container Platform 3.11.82 implementeert en bevat de volgende configuratieopties:

- **Master knooppunten**: Drie (3) Master knooppunten met configureerbare exemplaar typt.
- **Infra knooppunten**: Drie (3) Infra knooppunten met configureerbare exemplaar typt.
- **Knooppunten**: Het aantal knooppunten (tussen 1 en 9) en het exemplaartype kunnen worden geconfigureerd.
- **Schijftype**: Beheerde schijven worden gebruikt.
- **Netwerken**: Ondersteuning voor nieuwe of bestaande netwerk en aangepaste CIDR-bereik.
- **CNS**: CNS kan worden ingeschakeld.
- **Metrische gegevens**: Metrische gegevens kan worden ingeschakeld.
- **Logboekregistratie**: Logboekregistratie kan worden ingeschakeld.
- **Azure Cloud Provider**: Standaard ingeschakeld, kan worden uitgeschakeld.

Klik in de linkerbovenhoek van de Azure-portal, op **een resource maken**, voert u 'openshift containerplatform' in het zoekvak en drukt u op Enter.

   ![Nieuwe resource-zoekopdracht](media/openshift-marketplace-self-managed/ocp-search.png)  
<br>

De pagina met resultaten wordt geopend met **Red Hat OpenShift Container Platform Self-Managed** in de lijst. 

   ![Nieuwe resource zoekresultaat](media/openshift-marketplace-self-managed/ocp-searchresult.png)  
<br>

Klik op de aanbieding voor het weergeven van details van de aanbieding. Voor het implementeren van deze aanbieding, klikt u op **maken**. De gebruikersinterface in te voeren die nodig zijn parameters wordt weergegeven. Het eerste scherm de **basisbeginselen** blade.

   ![Pagina met de titel van aanbieding](media/openshift-marketplace-self-managed/ocp-titlepage.png)  
<br>

**Basisinstellingen**

Als u help op een van de invoerparameters die zijn opgegeven, Beweeg de muisaanwijzer over de ***ik*** naast de parameternaam van de.

Voer waarden in voor de invoerparameters en klikt u op **OK**.

| Invoerparameter | Beschrijving van de parameter |
|-----------------------|-----------------|
| VM-naam van de gebruiker met beheerdersrechten | De beheerder moet worden gemaakt op alle VM-exemplaren |
| SSH openbare sleutel voor de gebruiker met beheerdersrechten | Openbare SSH-sleutel gebruikt voor aanmelding bij de VM - mag geen een wachtwoordzin |
| Abonnement | Cluster implementeert in Azure-abonnement |
| Resourcegroep | Maak een nieuwe resourcegroep of Selecteer een bestaande lege resourcegroep voor de clusterresources |
| Locatie | Cluster implementeert in Azure-regio |

   ![Blade grondbeginselen bieden](media/openshift-marketplace-self-managed/ocp-basics.png)  
<br>

**Instellingen van de infrastructuur**

Voer waarden in voor de invoerparameters en klikt u op **OK**.

| Invoerparameter | Beschrijving van de parameter |
|-----------------------|-----------------|
| Voorvoegsel van OCP-Cluster | De beheerder moet worden gemaakt op alle VM-exemplaren |
| De grootte van de master-knooppunt | Accepteer de standaard-VM-grootte of klik op **grootte wijzigen** om te selecteren van een andere VM-grootte.  Selecteer de juiste VM-grootte voor uw werkbelasting |
| De grootte van de infrastructuur-knooppunt | Accepteer de standaard-VM-grootte of klik op **grootte wijzigen** om te selecteren van een andere VM-grootte.  Selecteer de juiste VM-grootte voor uw werkbelasting |
| Aantal knooppunten van de toepassing | Accepteer de standaard-VM-grootte of klik op **grootte wijzigen** om te selecteren van een andere VM-grootte.  Selecteer de juiste VM-grootte voor uw werkbelasting |
| De grootte van de toepassing-knooppunt | Accepteer de standaard-VM-grootte of klik op **grootte wijzigen** om te selecteren van een andere VM-grootte.  Selecteer de juiste VM-grootte voor uw werkbelasting |
| Grootte van de Host bastionhost | Accepteer de standaard-VM-grootte of klik op **grootte wijzigen** om te selecteren van een andere VM-grootte.  Selecteer de juiste VM-grootte voor uw werkbelasting |
| Nieuwe of bestaande virtuele netwerk | Maak een nieuw vNet (standaard) of een bestaand vNet gebruiken |
| Kies CIDR standaardinstellingen of IP-adresbereik (CIDR) aanpassen | Standaard CIDR-bereiken of selecteer accepteren **aangepaste IP-adresbereik** en voer de aangepaste CIDR-gegevens.  Standaardinstellingen wordt vNet maken met CIDR van 10.0.0.0/14, hoofd subnet met 10.1.0.0/16 infra subnet met 10.2.0.0/16 en reken- en cns subnet met 10.3.0.0/16 |
| De naam van de Key Vault-resourcegroep | De naam van de resourcegroep met de Key Vault |
| Key Vault-naam | De naam van de Key Vault waarin het geheim met de ssh persoonlijke sleutel.  Alleen alfanumerieke tekens en streepjes zijn toegestaan en tussen de 3 en 24 tekens bevatten |
| Geheime naam | De naam van het geheim met de ssh persoonlijke sleutel.  Alleen alfanumerieke tekens en streepjes zijn toegestaan |

   ![Blade infrastructuur bieden](media/openshift-marketplace-self-managed/ocp-inframain.png)  
<br>

**Formaat wijzigen**

Als u wilt een andere VM-grootte selecteren, klikt u op ***grootte wijzigen***.  Het venster van de selectie van virtuele machine wordt geopend.  Selecteer de VM-grootte en klik op **Selecteer**.

   ![VM-grootte selecteren](media/openshift-marketplace-self-managed/ocp-selectvmsize.png)  
<br>

**Bestaand virtueel netwerk**

| Invoerparameter | Beschrijving van de parameter |
|-----------------------|-----------------|
| Bestaande virtuele-netwerknaam | Naam van de bestaande vNet |
| Subnetnaam voor hoofdknooppunten | Naam van het bestaande subnet voor hoofdknooppunten.  Moet ten minste 16 IP-adressen bevatten en gaat u als volgt RFC 1918 |
| Subnetnaam voor infra knooppunten | Naam van bestaande subnet voor infra knooppunten.  Moet ten minste 32 IP-adressen bevatten en gaat u als volgt RFC 1918 |
| Subnetnaam voor reken- en cns knooppunten | Naam van het bestaande subnet voor reken- en cns knooppunten.  Moet ten minste 32 IP-adressen bevatten en gaat u als volgt RFC 1918 |
| Resourcegroep voor de bestaande Virtueelnetwerk | Naam van de resourcegroep met de bestaande vNet |

   ![Bestaande vnet infrastructuur bieden](media/openshift-marketplace-self-managed/ocp-existingvnet.png)  
<br>

**Aangepaste IP-bereik**

| Invoerparameter | Beschrijving van de parameter |
|-----------------------|-----------------|
| Adresbereik voor het virtuele netwerk | Aangepaste CIDR voor het vNet |
| Adresbereik voor het subnet met de hoofdknooppunten | Aangepaste Classless voor hoofd-subnet |
| Adresbereik voor het subnet met de infrastructuurknooppunten | Aangepaste CIDR voor infrastructuur-subnet |
| -Adresbereik voor subnet met daarin de reken- en cns knooppunten | Aangepaste CIDR voor de reken- en cns knooppunten |

   ![Bieden van aangepaste IP-adresbereik van infrastructuur](media/openshift-marketplace-self-managed/ocp-customiprange.png)  
<br>

**OpenShift Container Platform**

Voer waarden in voor de invoerparameters en klikt u op **OK**

| Invoerparameter | Beschrijving van de parameter |
|-----------------------|-----------------|
| OpenShift-beheerderswachtwoord | Wachtwoord voor de eerste OpenShift-gebruikersaccount.  Deze gebruiker worden ook de cluster-beheerder |
| OpenShift-beheerderswachtwoord bevestigen | Typ nogmaals het beheerderswachtwoord van OpenShift |
| Red Hat Doelabonnementbeheerder-gebruikersnaam | Gebruikersnaam voor toegang tot uw abonnement op Red Hat of organisatie-ID.  Deze referentie wordt gebruikt voor het registreren van de RHEL-instantie aan uw abonnement en niet door Microsoft en Red Hat worden opgeslagen |
| Red Hat Doelabonnementbeheerder gebruikerswachtwoord | Wachtwoord voor toegang tot uw abonnement op Red Hat of de activeringscode.  Deze referentie wordt gebruikt voor het registreren van de RHEL-instantie aan uw abonnement en niet door Microsoft en Red Hat worden opgeslagen |
| Red Hat Doelabonnementbeheerder OpenShift groeps-ID | ID van toepassingen met OpenShift Container Platform waar u recht op. Zorg ervoor dat u hebt onvoldoende rechten van OpenShift Container Platform voor de installatie van het cluster |
| Red Hat Doelabonnementbeheerder OpenShift groeps-ID voor Broker / model van knooppunten | Pool-ID die OpenShift Container Platform rechten voor Broker bevat / Master-knooppunten. Zorg ervoor dat u hebt onvoldoende rechten van OpenShift Container Platform voor de installatie van het cluster. Als geen broker / master groeps-ID, de groeps-ID voor de knooppunten van de toepassing invoeren |
| Azure-Cloud-Provider configureren | OpenShift voor het gebruik van Azure Cloud-Provider configureren. Dit is nodig als voor permanente volumes met behulp van Azure-schijf koppelen.  Standaard is Ja |
| Azure AD-Service-Principal-Client-ID-GUID | Azure AD-Service-Principal-Client-ID GUID - ook wel bekend als toepassings-id. Alleen nodig als Cloudprovider voor Azure configureren die zijn ingesteld op **Ja** |
| Azure AD-Service-Principal-ID Clientgeheim | Azure AD Service Principal Client ID Secret. Alleen nodig als Cloudprovider voor Azure configureren die zijn ingesteld op **Ja** |
 
   ![OpenShift-blade bieden](media/openshift-marketplace-self-managed/ocp-ocpmain.png)  
<br>

**Aanvullende instellingen**

De blade extra instellingen kan de configuratie van CNS voor glusterfs opslag, logboekregistratie en metrische gegevens Router Sub-domein.  De standaardwaarde van deze opties niet worden geïnstalleerd en nip.io gebruikt als het subdomein van de router voor testdoeleinden. Inschakelen van CNS installeert drie extra rekenknooppunten met drie extra gekoppelde schijven die als voor glusterfs schillen host.  

Voer waarden in voor de invoerparameters en klikt u op **OK**

| Invoerparameter | Beschrijving van de parameter |
|-----------------------|-----------------|
| Container systeemeigen opslag (CNS) configureren | Installaties CNS in de OpenShift cluster en als opslag in te schakelen. Standaard worden als Azure-Provider is uitgeschakeld |
| Cluster-logboekregistratie configureren | Installeert EFK-functionaliteit in het cluster.  Het formaat van infra knooppunten op de juiste wijze aan host EFK schillen |
| Metrische gegevens voor het Cluster configureren | Hawkular metrische gegevens installeert in het cluster van OpenShift.  Het formaat van infra knooppunten op de juiste wijze aan host Hawkular metrische gegevens schillen |
| Standaard Router subdomein | Selecteer nipio voor het testen of aangepaste in te voeren van uw eigen subdomein voor productie |
 
   ![Nieuwe aanbieding-blade](media/openshift-marketplace-self-managed/ocp-additionalmain.png)  
<br>

**Extra instellingen - Extra Parameters**

| Invoerparameter | Beschrijving van de parameter |
|-----------------------|-----------------|
| (CNS) Knooppuntgrootte | Accepteer de standaardgrootte van het knooppunt of selecteer **grootte wijzigen** om te selecteren van een nieuwe VM-grootte |
| Voer uw aangepaste subdomein | Het aangepaste routering domein moet worden gebruikt voor de weergave van toepassingen via de router op het cluster OpenShift.  Zorg ervoor dat de juiste wildcard-DNS-vermelding maken] |
 
   ![Bieden aanvullende cns installeren](media/openshift-marketplace-self-managed/ocp-additionalcnsall.png)  
<br>

**Samenvatting**

Validatie wordt uitgevoerd in deze fase moet worden gecontroleerd quotum voor kernen voldoende zijn voor het implementeren van het totale aantal virtuele machines die zijn geselecteerd voor het cluster.  Controleer de parameters die zijn ingevoerd.  Als de invoer geaccepteerd worden, klikt u op **OK** om door te gaan.

   ![De overzichtsblade van aanbieding](media/openshift-marketplace-self-managed/ocp-summary.png)  
<br>

**Kopen**

Controleer of de contactgegevens op de pagina kopen en klikt u op **aankoop** accepteer de gebruiksvoorwaarden en starten van de implementatie van het cluster OpenShift Container Platform.

   ![Aanbieding aankoop-blade](media/openshift-marketplace-self-managed/ocp-purchase.png)  
<br>


## <a name="connect-to-the-openshift-cluster"></a>Verbinding maken met het cluster OpenShift

Als de implementatie is voltooid, haalt u de verbinding in het gedeelte van de uitvoer van de implementatie. Verbinding maken met de console OpenShift met uw browser met behulp van de **OpenShift-Console URL**. U kunt ook SSH naar de bastionhost. Hieronder volgt een voorbeeld waarbij de beheerdersgebruikersnaam clusteradmin en het openbare IP voor bastionhost DNS FQDN bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com:

```bash
$ ssh clusteradmin@bastiondns4hawllzaavu6g.eastus.cloudapp.azure.com
```

## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de [az group delete](/cli/azure/group) opdracht voor het verwijderen van de resourcegroep, OpenShift-cluster en alle gerelateerde resources wanneer ze niet meer nodig zijn.

```azurecli 
az group delete --name openshiftrg
```

## <a name="next-steps"></a>Volgende stappen

- [Taken na de implementatie](./openshift-post-deployment.md)
- [OpenShift-implementatie in Azure oplossen](./openshift-troubleshooting.md)
- [Aan de slag met OpenShift Container Platform](https://docs.openshift.com/container-platform)


---
title: Een cluster die worden uitgevoerd op Windows met behulp van Windows-beveiliging beveiligen | Microsoft Docs
description: Informatie over het configureren van beveiliging van knooppunt-naar-knooppunt en client-naar-knooppunt op een zelfstandige cluster die worden uitgevoerd op Windows met behulp van Windows-beveiliging.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: chackdan
editor: ''
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: 394ba3b3b8189bbe96137e920745f7b8cdd1cd95
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666671"
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Een zelfstandige cluster beveiligen op Windows met behulp van Windows-beveiliging
Om te voorkomen dat onbevoegde toegang tot een Service Fabric-cluster, moet u het cluster beveiligen. Beveiliging is vooral belangrijk wanneer het cluster wordt uitgevoerd voor werkbelastingen voor productie. In dit artikel wordt beschreven hoe u knooppunt-naar-knooppunt en client-naar-knooppunt om beveiligingsbeleid te configureren met behulp van Windows-beveiliging in de *ClusterConfig.JSON* bestand.  Het proces komt overeen met de stap van de beveiliging configureren van [maken van een zelfstandige cluster die worden uitgevoerd op Windows](service-fabric-cluster-creation-for-windows-server.md). Zie voor meer informatie over hoe Service Fabric Windows-beveiliging gebruikt [clusterbeveiligingsscenario's](service-fabric-cluster-security.md).

> [!NOTE]
> U moet rekening houden met de selectie van de beveiliging van knooppunt-naar-knooppunt zorgvuldig omdat er geen clusterupgrade van één beveiliging keuze naar een andere. De beveiliging als selectie wilt wijzigen, moet u het volledige cluster opnieuw.
>
>

## <a name="configure-windows-security-using-gmsa"></a>Windows-beveiliging met behulp van gMSA configureren  
Het voorbeeld *ClusterConfig.gMSA.Windows.MultiMachine.JSON* configuratiebestand is gedownload met de [Microsoft.Azure.ServiceFabric.WindowsServer.\< versie > .zip](https://go.microsoft.com/fwlink/?LinkId=730690) zelfstandige cluster pakket bevat een sjabloon voor het configureren van Windows-beveiliging via [groep beheerde serviceaccounts (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):  

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {  
        "ClustergMSAIdentity": "[gMSA Identity]",
        "ClusterSPN": "[Registered SPN for the gMSA account]",
        "ClientIdentities": [
            {
                "Identity": "domain\\username",
                "IsAdmin": true
            }
        ]
    }
}
```

| **Configuratie-instelling** | **Beschrijving** |
| --- | --- |
| ClusterCredentialType |Ingesteld op *Windows* naar het Windows-beveiliging inschakelen voor communicatie tussen knooppunten.  | 
| ServerCredentialType |Ingesteld op *Windows* om in te schakelen van Windows-beveiliging voor de communicatie van client-knooppunt. |
| WindowsIdentities |Het cluster en de client-id's bevat. |
| ClustergMSAIdentity |Hiermee configureert u beveiliging van knooppunt-naar-knooppunt. Een groep beheerd serviceaccount. |
| ClusterSPN |Geregistreerde SPN voor een beheerd serviceaccount|
| ClientIdentities |Hiermee configureert u beveiliging van de client-naar-knooppunt. Een matrix van gebruikersaccounts van de client. |
| Identiteit |Voeg de domeingebruiker, domein\gebruikersnaam voor de clientidentiteit. |
| IsAdmin |Ingesteld op true om op te geven dat de domeingebruiker clienttoegang als beheerder of ONWAAR voor gebruiker clienttoegang heeft. |

> [!NOTE]
> ClustergMSAIdentity waarde kan niet de domeinnaam bevatten en mag alleen bestaan uit de groepsnaam van de beheerde service-account. DAT WIL ZEGGEN "mysfgmsa" juist is, en beide "mijndomein / / mysfgmsa ' of 'mysfgmsa@mydomain' zijn ongeldig; als het domein wordt geïmpliceerd door de hostmachine.

[Knooppunt voor beveiliging van knooppunt](service-fabric-cluster-security.md#node-to-node-security) is geconfigureerd door in te stellen **ClustergMSAIdentity** wanneer service fabric moet worden uitgevoerd onder gMSA. Het samenstellen van vertrouwensrelaties tussen knooppunten, moeten ze worden gemaakt op de hoogte van elkaar. Dit kan op twee verschillende manieren worden bereikt: Geef de groep beheerd serviceaccount met alle knooppunten in het cluster of de Machinegroep met alle knooppunten in het cluster opgeven. Wordt aangeraden met behulp van de [groep beheerde serviceaccounts (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) benadering, met name voor grotere clusters (meer dan 10 knooppunten) of voor clusters die waarschijnlijk vergroten of verkleinen.  
Deze benadering is niet vereist voor het maken van een domeingroep waarvoor clusterbeheerders rechten voor het toevoegen en verwijderen van leden hebben gekregen. Deze accounts zijn ook nuttig voor wachtwoorden worden automatisch beheerd. Zie voor meer informatie, [aan de slag met beheerde serviceaccounts voor groepen](https://technet.microsoft.com/library/jj128431.aspx).  
 
[Client en knooppunt security](service-fabric-cluster-security.md#client-to-node-security) is geconfigureerd met behulp van **ClientIdentities**. Als u wilt de vertrouwensrelatie tussen een client en het cluster, moet u het cluster als u wilt weten welke client identiteiten die kunnen worden vertrouwd. Dit kan op twee verschillende manieren worden gedaan: Geef de groep Domeingebruikers die u kunnen verbinding maken, of knooppunt met de domeingebruikers die verbinding kunnen maken. Service Fabric ondersteunt twee verschillende typen voor clients die zijn verbonden met een Service Fabric-cluster: beheerder en gebruiker. Toegangsbeheer biedt de mogelijkheid voor de Clusterbeheerder om te beperken van toegang tot bepaalde typen bewerkingen voor een cluster voor verschillende groepen gebruikers, waardoor het cluster beter te beveiligen.  Beheerders hebben volledige toegang tot de mogelijkheden voor beheer (met inbegrip van de mogelijkheden voor lezen/schrijven). Gebruikers hebben standaard alleen leestoegang tot de mogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services. Zie voor meer informatie over toegangsbeheer [op rollen gebaseerd toegangsbeheer voor Service Fabric-clients](service-fabric-cluster-security-roles.md).  
 
Het volgende voorbeeld **security** sectie configureert u Windows-beveiliging met behulp van gMSA en Hiermee wordt aangegeven dat de machines in *ServiceFabric.clusterA.contoso.com* gMSA maken deel uit van het cluster en die  *CONTOSO\usera* admin-clienttoegang heeft:  
  
```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",
        "ClusterSPN" : "http/servicefabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
}
```
  
## <a name="configure-windows-security-using-a-machine-group"></a>Windows-beveiliging met behulp van een Machinegroep configureren  
Dit model wordt afgeschaft. De aanbeveling is het gebruik van gMSA zoals hierboven beschreven. Het voorbeeld *ClusterConfig.Windows.MultiMachine.JSON* configuratiebestand is gedownload met de [Microsoft.Azure.ServiceFabric.WindowsServer.\< versie > .zip](https://go.microsoft.com/fwlink/?LinkId=730690) zelfstandige cluster pakket bevat een sjabloon voor het configureren van Windows-beveiliging.  Windows-beveiliging is geconfigureerd in de **eigenschappen** sectie: 

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "[domain\machinegroup]",
        "ClientIdentities": [{
            "Identity": "[domain\username]",
            "IsAdmin": true
        }]
    }
}
```

| **Configuratie-instelling** | **Beschrijving** |
| --- | --- |
| ClusterCredentialType |Ingesteld op *Windows* naar het Windows-beveiliging inschakelen voor communicatie tussen knooppunten.  |
| ServerCredentialType |Ingesteld op *Windows* om in te schakelen van Windows-beveiliging voor de communicatie van client-knooppunt. |
| WindowsIdentities |Het cluster en de client-id's bevat. |
| ClusterIdentity |Gebruik een machine-groepsnaam domain\machinegroup, beveiliging van knooppunt-naar-knooppunt configureren. |
| ClientIdentities |Hiermee configureert u beveiliging van de client-naar-knooppunt. Een matrix van gebruikersaccounts van de client. |  
| Identiteit |Voeg de domeingebruiker, domein\gebruikersnaam voor de clientidentiteit. |  
| IsAdmin |Ingesteld op true om op te geven dat de domeingebruiker clienttoegang als beheerder of ONWAAR voor gebruiker clienttoegang heeft. |  

[Knooppunt voor beveiliging van knooppunt](service-fabric-cluster-security.md#node-to-node-security) is geconfigureerd met behulp van instelling **ClusterIdentity** als u wilt gebruiken van een Machinegroep binnen een Active Directory-domein. Zie voor meer informatie, [maken van een Machine-groep in Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).

[Beveiliging van de client-naar-knooppunt](service-fabric-cluster-security.md#client-to-node-security) is geconfigureerd met behulp van **ClientIdentities**. Als u wilt de vertrouwensrelatie tussen een client en het cluster, moet u het cluster als u wilt weten van de client-identiteiten die kan worden vertrouwd door het cluster configureren. U kunt vertrouwen op twee verschillende manieren:

- Geef de groep Domeingebruikers die verbinding kunnen maken.
- Geef de domein-knooppunt-gebruikers die verbinding kunnen maken.

Service Fabric ondersteunt twee verschillende typen voor clients die zijn verbonden met een Service Fabric-cluster: beheerder en gebruiker. Access control maakt de Clusterbeheerder om te beperken van toegang tot bepaalde typen bewerkingen voor een cluster voor verschillende groepen gebruikers, waardoor het cluster beter te beveiligen.  Beheerders hebben volledige toegang tot de mogelijkheden voor beheer (met inbegrip van de mogelijkheden voor lezen/schrijven). Gebruikers hebben standaard alleen leestoegang tot de mogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.  

Het volgende voorbeeld **security** sectie Windows-beveiliging configureert, geeft aan dat de machines in *ServiceFabric/clusterA.contoso.com* deel uitmaken van het cluster en geeft aan dat *CONTOSO\usera* admin-clienttoegang heeft:

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> Service Fabric mogen niet worden geïmplementeerd op een domeincontroller. Zorg ervoor dat ClusterConfig.json niet het IP-adres van de domeincontroller zijn wanneer u een computergroep of beheerd serviceaccount (gMSA).
>
>

## <a name="next-steps"></a>Volgende stappen
Na het configureren van Windows-beveiliging in de *ClusterConfig.JSON* bestand, hervat u het proces voor het cluster maken in [maken van een zelfstandige cluster die worden uitgevoerd op Windows](service-fabric-cluster-creation-for-windows-server.md).

Voor meer informatie over de beveiliging, beveiliging van de client-naar-knooppunt en op rollen gebaseerd toegangsbeheer, Zie hoe knooppunt-naar-knooppunt [clusterbeveiligingsscenario's](service-fabric-cluster-security.md).

Zie [verbinding maken met een beveiligd cluster](service-fabric-connect-to-secure-cluster.md) voor voorbeelden van de verbinding te maken met behulp van PowerShell of FabricClient.

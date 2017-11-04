---
title: Beveiligen van een cluster met Windows met behulp van Windows-beveiliging | Microsoft Docs
description: Informatie over het configureren van client-naar-knooppunt en knooppunt naar beveiliging op een zelfstandige cluster met Windows met behulp van Windows-beveiliging.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: e093a631b0cf81195981a8e3d345504ebce02723
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a>Een zelfstandige cluster op Windows beveiligen met behulp van Windows-beveiliging
Om te voorkomen dat onbevoegde toegang tot een Service Fabric-cluster, moet u het cluster te beveiligen. Beveiliging is vooral belangrijk bij het cluster wordt uitgevoerd voor productie-workloads. Dit artikel wordt beschreven hoe u de client-naar-knooppunt en knooppunt naar beveiliging configureren met behulp van Windows-beveiliging in de *ClusterConfig.JSON* bestand.  Het proces komt overeen met de stap van de beveiliging configureren van [maken van een zelfstandige cluster waarop Windows](service-fabric-cluster-creation-for-windows-server.md). Zie voor meer informatie over hoe Service Fabric Windows-beveiliging gebruikt [security scenario's](service-fabric-cluster-security.md).

> [!NOTE]
> Omdat er geen upgrade van één beveiliging keuze van de cluster naar een andere, moet u de selectie van knooppunt naar beveiliging zorgvuldig overwegen. De beveiliging als selectie wilt wijzigen, moet u opnieuw opbouwen van het volledige cluster.
>
>

## <a name="configure-windows-security-using-gmsa"></a>Windows-beveiliging met behulp van gMSA configureren  
Het voorbeeld *ClusterConfig.gMSA.Windows.MultiMachine.JSON* configuratiebestand is gedownload met de [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) zelfstandige clusterpakket bevat een sjabloon voor het configureren van Windows-beveiliging gebruikt [groep beheerde serviceaccounts (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):  

```  
"security": {  
            "WindowsIdentities": {  
                "ClustergMSAIdentity": "accountname@fqdn"  
                "ClusterSPN": "fqdn"  
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
| WindowsIdentities |Het cluster en de client-id's bevat. |  
| ClustergMSAIdentity |Hiermee configureert u een knooppunt naar beveiliging. Een groep beheerd serviceaccount. |  
| ClusterSPN |FQDN-SPN voor de gMSA-account|  
| ClientIdentities |Hiermee configureert u clientknooppunt beveiliging. Een matrix van gebruikersaccounts van de client. |  
| Identiteit |De client-id, een domeingebruiker. |  
| IsAdmin |Waar geeft aan dat de domeingebruiker beheerder clienttoegang, false voor clienttoegang van de gebruiker heeft. |  
  
[Knooppunt voor de beveiliging van knooppunt](service-fabric-cluster-security.md#node-to-node-security) is geconfigureerd door in te stellen **ClustergMSAIdentity** wanneer het service fabric moet worden uitgevoerd onder gMSA. Om te kunnen maken van vertrouwensrelaties tussen knooppunten, moeten ze worden aangebracht op de hoogte van elkaar. Dit op twee verschillende manieren kunt worden bereikt: de groep beheerd serviceaccount gebruiken met alle knooppunten in het cluster opgeven, of de Machinegroep met alle knooppunten in het cluster. Wij raden met behulp van de [groep beheerde serviceaccounts (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) benadering, met name voor grotere clusters (meer dan 10 knooppunten) of voor clusters die waarschijnlijk het vergroten of verkleinen.  
Deze aanpak is niet vereist voor het maken van een domeingroep waarvoor clusterbeheerders rechten voor het toevoegen en verwijderen van leden hebben gekregen. Deze accounts zijn ook nuttig voor het automatisch wachtwoordbeheer. Zie voor meer informatie [aan de slag met beheerde serviceaccounts voor groepen](http://technet.microsoft.com/library/jj128431.aspx).  
 
[Client naar het knooppunt beveiliging](service-fabric-cluster-security.md#client-to-node-security) is geconfigureerd met **ClientIdentities**. Om een vertrouwensrelatie tussen een client en het cluster, moet u het cluster om te weten welke client identiteiten die kunnen worden vertrouwd. Dit kan op twee verschillende manieren worden gedaan: Geef de groep Domeingebruikers die u kunnen verbinding maken, of het knooppunt domeingebruikers die verbinding kunnen maken. Service Fabric ondersteunt twee verschillende toegangsrechten besturingselementtypen voor clients die zijn verbonden met een Service Fabric-cluster: beheerder en gebruiker. Toegangsbeheer biedt de mogelijkheid voor de Clusterbeheerder om te beperken van toegang tot bepaalde soorten clusterbewerkingen voor verschillende groepen gebruikers, zodat het cluster beter te beveiligen.  Beheerders hebben volledige toegang tot de beheerfuncties (inclusief lezen/schrijven-mogelijkheden). Gebruikers hebben standaard alleen leestoegang tot beheermogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services. Zie voor meer informatie over toegangsbeheer [toegangsbeheer voor Service Fabric-clients op basis van rollen](service-fabric-cluster-security-roles.md).  
 
Het volgende voorbeeld **beveiliging** sectie configureert u Windows-beveiliging met behulp van gMSA en geeft aan dat de computers in *ServiceFabric.clusterA.contoso.com* gMSA maken deel uit van het cluster en die  *CONTOSO\usera* admin clienttoegang heeft:  
  
```  
"security": {  
    "WindowsIdentities": {  
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",  
        "ClusterSPN" : "clusterA.contoso.com",  
        "ClientIdentities": [{  
            "Identity": "CONTOSO\\usera",  
            "IsAdmin": true  
        }]  
    }  
}  
```  
  
## <a name="configure-windows-security-using-a-machine-group"></a>Windows-beveiliging met behulp van een machine-beheergroep configureren  
Het voorbeeld *ClusterConfig.Windows.MultiMachine.JSON* configuratiebestand is gedownload met de [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. ZIP](http://go.microsoft.com/fwlink/?LinkId=730690) zelfstandige clusterpakket bevat een sjabloon voor het configureren van Windows-beveiliging.  Windows-beveiliging is geconfigureerd in de **eigenschappen** sectie: 

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
| ClusterCredentialType |**ClusterCredentialType** is ingesteld op *Windows* als ClusterIdentity is opgegeven voor een Active Directory-groep computernaam. |  
| ServerCredentialType |Ingesteld op *Windows* Windows-beveiliging inschakelen voor clients.<br /><br />Dit betekent dat de clients van het cluster en het cluster zelf worden uitgevoerd binnen een Active Directory-domein. |  
| WindowsIdentities |Het cluster en de client-id's bevat. |  
| ClusterIdentity |Gebruik een machine-groepsnaam domain\machinegroup, knooppunt naar beveiliging configureren. |  
| ClientIdentities |Hiermee configureert u clientknooppunt beveiliging. Een matrix van gebruikersaccounts van de client. |  
| Identiteit |Voeg de domeingebruiker, domein\gebruikersnaam voor de identiteit van de client. |  
| IsAdmin |Ingesteld op true om op te geven dat de domeingebruiker clienttoegang als beheerder of ONWAAR voor clienttoegang van de gebruiker heeft. |  

[Knooppunt voor de beveiliging van knooppunt](service-fabric-cluster-security.md#node-to-node-security) is geconfigureerd met behulp van instelling **ClusterIdentity** als u wilt gebruiken van een Machinegroep binnen een Active Directory-domein. Zie voor meer informatie [maken van een Machine-groep in Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).

[Beveiliging van de client naar het knooppunt](service-fabric-cluster-security.md#client-to-node-security) is geconfigureerd met behulp van **ClientIdentities**. Als u wilt een vertrouwensrelatie tussen een client en het cluster, moet u het cluster om te weten van de client-identiteiten die kan worden vertrouwd door het cluster configureren. U kunt een vertrouwensrelatie in twee verschillende manieren:

- Geef de groep Domeingebruikers die verbinding kunnen maken.
- Geef de domein-knooppunt gebruikers die verbinding kunnen maken.

Service Fabric ondersteunt twee verschillende toegangsrechten besturingselementtypen voor clients die zijn verbonden met een Service Fabric-cluster: beheerder en gebruiker. Toegangsbeheer kan de Clusterbeheerder te beperken van toegang tot bepaalde soorten clusterbewerkingen voor verschillende groepen gebruikers, waardoor het cluster beter te beveiligen.  Beheerders hebben volledige toegang tot de beheerfuncties (inclusief lezen/schrijven-mogelijkheden). Gebruikers hebben standaard alleen leestoegang tot beheermogelijkheden (bijvoorbeeld querymogelijkheden) en de mogelijkheid om op te lossen, toepassingen en services.  

Het volgende voorbeeld **beveiliging** sectie Windows-beveiliging configureert, geeft aan dat de computers in *ServiceFabric/clusterA.contoso.com* deel uitmaken van het cluster en geeft aan dat *CONTOSO\usera* admin clienttoegang heeft:

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
> Service Fabric moeten niet worden geïmplementeerd op een domeincontroller. Zorg ervoor dat ClusterConfig.json niet opnemen van het IP-adres van de domeincontroller bij gebruik van een Machinegroep of groep beheerde serviceaccounts (gMSA).
>
>

## <a name="next-steps"></a>Volgende stappen
Na het configureren van Windows-beveiliging in de *ClusterConfig.JSON* bestand, doorgaan met het maken van de cluster in [maken van een zelfstandige cluster waarop Windows](service-fabric-cluster-creation-for-windows-server.md).

Voor meer informatie over hoe naar knooppunten beveiliging, client-naar-node beveiliging en op rollen gebaseerde toegangsbeheer, Zie [security scenario's](service-fabric-cluster-security.md).

Zie [verbinding maken met een beveiligde cluster](service-fabric-connect-to-secure-cluster.md) voor voorbeelden van verbinding maken met behulp van PowerShell of FabricClient.

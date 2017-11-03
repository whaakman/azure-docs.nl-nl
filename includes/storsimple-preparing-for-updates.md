<!--author=jgerend last changed: 03/16/16-->

## <a name="preparing-for-updates"></a>Voorbereiden voor updates
U moet de volgende stappen uitvoeren voordat u het scannen en de update toepassen:

1. Een cloud momentopname van de gegevens van het apparaat.
2. Controleer of uw vaste IP-adressen controller routeerbaar zijn en verbinding kunnen maken met Internet. Deze vaste IP-adressen wordt gebruikt voor het service-updates op uw apparaat. U kunt dit testen met de volgende cmdlet wordt uitgevoerd op elke domeincontroller van de Windows PowerShell-interface van het apparaat:
   
     `Test-Connection -Source <Fixed IP of your device controller> -Destination <Any IP or computer name outside of datacenter network> `
   
    **Voorbeelduitvoer voor Test-Connection wanneer vaste IP-adressen verbinding met Internet maken kan**

        Controller0>Test-Connection -Source 10.126.173.91 -Destination bing.com

        Source      Destination     IPV4Address      IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200
        HCSNODE0  bing.com        204.79.197.200

        Controller0>Test-Connection -Source 10.126.173.91 -Destination  204.79.197.200

        Source      Destination       IPV4Address    IPV6Address
        ----------------- -----------  -----------
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200
        HCSNODE0  204.79.197.200  204.79.197.200

Nadat u deze handmatig controles vooraf hebt voltooid, kunt u doorgaan om te scannen en de updates worden geïnstalleerd.


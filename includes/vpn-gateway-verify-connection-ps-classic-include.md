U kunt controleren of de verbinding met de cmdlet 'Get-AzureVNetConnection' is voltooid.

1. Gebruik het volgende cmdlet-voorbeeld om de waarden aan te passen aan uw eigen waarden. De naam van het virtuele netwerk moet tussen aanhalingstekens als deze spaties bevat.

  ```powershell
  Get-AzureVNetConnection "Group ClassicRG ClassicVNet"
  ```
2. Bekijk de waarden nadat de cmdlet is voltooid. In het volgende voorbeeld wordt de status van de connectiviteit wordt weergegeven als 'Verbonden' en ziet u inkomende en uitgaande bytes.

        ConnectivityState         : Connected
        EgressBytesTransferred    : 181664
        IngressBytesTransferred   : 182080
        LastConnectionEstablished : 1/7/2016 12:40:54 AM
        LastEventID               : 24401
        LastEventMessage          : The connectivity state for the local network site 'RMVNetLocal' changed from Connecting to
                                    Connected.
        LastEventTimeStamp        : 1/7/2016 12:40:54 AM
        LocalNetworkSiteName      : RMVNetLocal
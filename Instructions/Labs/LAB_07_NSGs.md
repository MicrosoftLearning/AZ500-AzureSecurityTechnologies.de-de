---
lab:
  title: 07 – Einrichten von Netzwerk- und Anwendungssicherheitsgruppen
  module: Module 02 - Implement Platform Protection
ms.openlocfilehash: 5446c0927ee81ee63b366554d2cef03be4a6fe1c
ms.sourcegitcommit: 2eb153f2856445e5afaa218a012cb92e3d48f24b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/16/2021
ms.locfileid: "132625648"
---
# <a name="lab-07-network-security-groups-and-application-security-groups"></a>Lab 07: Einrichten von Netzwerk- und Anwendungssicherheitsgruppen
# <a name="student-lab-manual"></a>Lab-Handbuch für Kursteilnehmer

## <a name="lab-scenario"></a>Labszenario

Sie wurden aufgefordert, die virtuelle Netzwerkinfrastruktur Ihrer Organisation zu implementieren und zu testen, um sicherzustellen, dass sie ordnungsgemäß funktioniert. Dies gilt insbesondere für:

- Die Organisation verfügt über zwei Gruppen von Servern: Webserver und Verwaltungsserver.
- Jede Gruppe von Servern sollte sich in einer eigenen Anwendungssicherheitsgruppe befinden. 
- Sie sollten in der Lage sein, RDP auf die Verwaltungsserver, aber nicht auf die Webserver zu übertragen.
- Auf den Webservern sollte die IIS-Webseite angezeigt werden, wenn über das Internet darauf zugegriffen wird. 
- Regeln für Netzwerksicherheitsgruppen sollten verwendet werden, um den Netzwerkzugriff zu steuern. 

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## <a name="lab-objectives"></a>Ziele des Labs

In diesem Lab führen Sie die folgenden Übungen aus:

- Übung 1: Erstellen der virtuellen Netzwerkinfrastruktur
- Übung 2: Bereitstellen virtueller Computer und Testen der Netzwerkfilter

### <a name="exercise-1-create-the-virtual-networking-infrastructure"></a>Übung 1: Erstellen der virtuellen Netzwerkinfrastruktur

### <a name="estimated-timing-20-minutes"></a>Geschätzte Zeit: 20 Minuten

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Erstellen eines virtuellen Netzwerks mit einem Subnetz.
- Aufgabe 2: Erstellen von zwei Anwendungssicherheitsgruppen.
- Aufgabe 3: Erstellen einer Netzwerksicherheitsgruppe und Zuordnen der NSG zum virtuellen Netzwerksubnetz.
- Aufgabe 4: Erstellen von NSG-Sicherheitsregeln für den gesamten eingehenden Datenverkehr an Webserver und RDP an die Verwaltungsserver.

#### <a name="task-1--create-a-virtual-network"></a>Aufgabe 1: Erstellen eines virtuellen Netzwerks

In dieser Aufgabe erstellen Sie ein virtuelles Netzwerk für die Verwendung mit den Netzwerk- und Anwendungssicherheitsgruppen. 

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, das über die Rolle „Besitzer“ oder „Mitwirkender“ in dem Azure-Abonnement verfügt, das Sie für dieses Lab verwenden.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Virtuelle Netzwerke** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** auf **+ Erstellen**.

1. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** auf der Registerkarte **Grundeinstellungen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen), und klicken Sie auf **Weiter: IP-Adressen**:

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden|
    |Resource group|Klicken Sie auf **Neu erstellen**, und geben Sie den Namen **AZ500LAB07** ein.|
    |Name|**myVirtualNetwork**|
    |Region|**USA, Osten**|

1. Legen Sie auf dem Blatt **Virtuelles Netzwerk erstellen** auf der Registerkarte **IP-Adressen** den **IPv4-Adressraum** auf **10.0.0.0/16** fest. Klicken Sie, falls erforderlich, in der Spalte **Subnetzname** auf **Standard**, geben Sie auf dem Blatt **Subnetz bearbeiten** die folgenden Einstellungen an, und klicken Sie dann auf **Speichern**:

    |Einstellung|Wert|
    |---|---|
    |Subnetzname|**default**|
    |Subnetzadressbereich|**10.0.0.0/24**|

1. Klicken Sie anschließend auf dem Blatt **Virtuelles Netzwerk erstellen** auf der Registerkarte **IP-Adressen** auf **Überprüfen + erstellen**.

1. Klicken Sie auf dem Blatt **Virtuelles Netzwerk erstellen** auf der Registerkarte **Überprüfen + erstellen** auf **Erstellen**.

#### <a name="task-2--create-application-security-groups"></a>Aufgabe 2: Erstellen von Anwendungssicherheitsgruppen

In dieser Aufgabe erstellen Sie eine Anwendungssicherheitsgruppe.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Anwendungssicherheitsgruppen** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Anwendungssicherheitsgruppen** auf **+ Erstellen**.

1. Geben Sie die folgenden Einstellungen auf der Registerkarte **Grundlagen** des Blatts **Anwendungssicherheitsgruppe erstellen** an: 

    |Einstellung|Wert|
    |---|---|
    |Resource group|**AZ500LAB07**|
    |Name|**myAsgWebServers**|
    |Region|**USA, Osten**|

    >**Hinweis:** Diese Gruppe ist für die Webserver vorgesehen.

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

1. Navigieren Sie zurück zum Blatt **Anwendungssicherheitsgruppen** und klicken Sie auf **+ Erstellen**.

1. Geben Sie die folgenden Einstellungen auf der Registerkarte **Grundlagen** des Blatts **Anwendungssicherheitsgruppe erstellen** an: 

    |Einstellung|Wert|
    |---|---|
    |Resource group|**AZ500LAB07**|
    |Name|**myAsgMgmtServers**|
    |Region|**USA, Osten**|

    >**Hinweis:** Diese Gruppe ist für die Verwaltungsserver vorgesehen.

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

#### <a name="task-3--create-a-network-security-group-and-associate-the-nsg-to-the-subnet"></a>Aufgabe 3: Erstellen einer Netzwerksicherheitsgruppe und Zuordnen der NSG zum Subnetz

In dieser Aufgabe erstellen Sie eine Netzwerksicherheitsgruppe. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Netzwerksicherheitsgruppen** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Netzwerksicherheitsgruppen** auf **+ Erstellen**.

1. Geben Sie auf dem Blatt **Netzwerksicherheitsgruppe erstellen** auf der Registerkarte **Grundeinstellungen** die folgenden Einstellungen an: 

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden|
    |Resource group|**AZ500LAB07**|
    |Name|**myNsg**|
    |Region|**USA, Osten**|

1. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Netzwerksicherheitsgruppen**, und klicken Sie auf den Eintrag **myNsg**.

1. Klicken Sie auf dem Blatt **myNsg** im Abschnitt **Einstellungen** auf **Subnetze** und dann auf **+ Zuordnen**. 

1. Geben Sie auf dem Blatt **Subnetz zuordnen** die folgenden Einstellungen an, und klicken Sie auf **OK**:

    |Einstellung|Wert|
    |---|---|
    |Virtuelles Netzwerk|**myVirtualNetwork**|
    |Subnet|**default**|

#### <a name="task-4-create-inbound-nsg-security-rules-to-all-traffic-to-web-servers-and-rdp-to-the-management-servers"></a>Aufgabe 4: Erstellen von NSG-Sicherheitsregeln für den gesamten eingehenden Datenverkehr an Webserver und RDP an die Verwaltungsserver. 

1. Klicken Sie auf dem Blatt **myNsg** im Abschnitt **Einstellungen** auf **Sicherheitsregeln für eingehenden Datenverkehr**.

1. Überprüfen Sie die Standardsicherheitsregeln für eingehenden Datenverkehr aus, und klicken Sie auf **+ Hinzufügen**.

1. Geben Sie auf dem Blatt **Sicherheitsregel für eingehenden Datenverkehr hinzufügen** die folgenden Einstellungen an, um TCP-Ports 80 und 443 für die Anwendungssicherheitsgruppe **myAsgWebServers** zuzulassen (übernehmen Sie für alle anderen Werte die Standardwerte): 

    |Einstellung|Wert|
    |---|---|
    |Destination|Wählen Sie in der Dropdownliste **Anwendungssicherheitsgruppe** aus, und klicken Sie dann auf **myAsgWebServers**.|
    |Zielportbereiche|**80, 443**|
    |Protokoll|**TCP**|
    |Priority|**100**|                                                    
    |Name|**Allow-Web-All**|

1. Klicken Sie auf dem Blatt **Sicherheitsregel für eingehenden Datenverkehr hinzufügen** auf **Hinzufügen**, um die neue Regel für eingehenden Datenverkehr zu erstellen. 

1. Klicken Sie auf dem Blatt **myNsg** im Abschnitt **Einstellungen** auf **Sicherheitsregeln für eingehenden Datenverkehr**, und klicken Sie dann auf **+ Hinzufügen**.

1. Geben Sie auf dem Blatt **Sicherheitsregel für eingehenden Datenverkehr hinzufügen** die folgenden Einstellungen an, um den RDP-Port (TCP 3389) für die Anwendungssicherheitsgruppe **myAsgMgmtServers** zuzulassen (übernehmen Sie für alle anderen Werte die Standardwerte): 

    |Einstellung|Wert|
    |---|---|
    |Destination|Wählen Sie in der Dropdownliste **Anwendungssicherheitsgruppe** aus, und klicken Sie dann auf **myAsgMgmtServers**.|
    |Zielportbereiche|**3389**|
    |Protokoll|**TCP**|
    |Priority|**110**|                                                    
    |Name|**Allow-RDP-All**|

1. Klicken Sie auf dem Blatt **Sicherheitsregel für eingehenden Datenverkehr hinzufügen** auf **Hinzufügen**, um die neue Regel für eingehenden Datenverkehr zu erstellen. 

> Ergebnis: Sie haben ein virtuelles Netzwerk, Netzwerksicherheit mit Sicherheitsregeln für eingehenden Datenverkehr und zwei Anwendungssicherheitsgruppen bereitgestellt. 

### <a name="exercise-2-deploy-virtual-machines-and-test-network-filters"></a>Übung 2: Bereitstellen virtueller Computer und Testen der Netzwerkfilter

### <a name="estimated-timing-25-minutes"></a>Geschätzte Zeit: 25 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Erstellen eines virtuellen Computers, der als Webserver verwendet werden soll.
- Aufgabe 2: Erstellen eines virtuellen Computers, der als Verwaltungsserver verwendet werden soll. 
- Aufgabe 3: Zuordnen der einzelnen Netzwerkschnittstellen virtueller Computer zur entsprechenden Anwendungssicherheitsgruppe.
- Aufgabe 4: Testen der Filterung des Netzwerkdatenverkehrs.

#### <a name="task-1-create-a-virtual-machine-to-use-as-a-web-server"></a>Aufgabe 1: Erstellen eines virtuellen Computers, der als Webserver verwendet werden soll.

In dieser Aufgabe erstellen Sie einen virtuellen Computer, der als Webserver verwendet werden soll.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Virtuelle Computer** ein, und drücken Sie die **EINGABETASTE**.

1. Klicken Sie auf dem Blatt **Virtuelle Computer** auf **+ Erstellen**, und klicken Sie in der Dropdownliste auf **+ Virtueller Computer**.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Virtuellen Computer erstellen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

   |Einstellung|Wert|
   |---|---|
   |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden|
   |Resource group|**AZ500LAB07**|
   |Name des virtuellen Computers|**myVmWeb**|
   |Region|**(USA) USA, Osten**|
   |Image|**Windows Server 2019 Datacenter, Gen 2**|
   |Size|**Standard D2s v3**|
   |Username|**Kursteilnehmer**|
   |Kennwort|**Pa55w.rd1234**|
   |Öffentliche Eingangsports|**None**|
   |Sie verfügen bereits über eine Windows Server-Lizenz|**Nein**|

    >**Hinweis**: Für öffentliche eingehende Ports verwenden wir die vorab erstellte Netzwerksicherheitsgruppe. 

1. Klicken Sie auf **Weiter: Datenträger >** , und legen Sie auf dem Blatt **Virtuellen Computer erstellen** auf der Registerkarte **Datenträger** den **Typ des Betriebssystemdatenträgers** auf **HDD Standard** fest, und klicken Sie dann auf **Weiter: Netzwerk >** .

1. Wählen Sie auf der Registerkarte **Netzwerk** des Blatts **Virtuellen Computer erstellen** das zuvor erstellte Netzwerk **myVirtualNetwork** aus.

1. Wählen Sie unter **NIC-Netzwerksicherheitsgruppe** die Option **Keine** aus.

1. Klicken Sie auf **Weiter: Verwaltung >** , und überprüfen Sie auf der Registerkarte **Verwaltung** des Blatts **Virtuellen Computer erstellen** die folgende Einstellung:

   |Einstellung|Wert|
   |---|---|
   |Startdiagnose|**Mit verwaltetem Speicherkonto aktiviert (empfohlen)**|

1. Klicken Sie auf **Überprüfen + erstellen**. Vergewissern Sie sich auf dem Blatt **Überprüfen + erstellen**, dass die Überprüfung erfolgreich war, und klicken Sie auf **Erstellen**.

#### <a name="task-2-create-a-virtual-machine-to-use-as-a-management-server"></a>Aufgabe 2: Erstellen eines virtuellen Computers, der als Verwaltungsserver verwendet werden soll. 

In dieser Aufgabe erstellen Sie einen virtuellen Computer, der als Verwaltungsserver verwendet werden soll.

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Virtuelle Computer**, klicken Sie auf **+ Erstellen** und dann in der Dropdownliste auf **+ Virtueller Computer**.

1. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Virtuellen Computer erstellen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

   |Einstellung|Wert|
   |---|---|
   |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden|
   |Resource group|**AZ500LAB07**|
   |Name des virtuellen Computers|**myVMMgmt**|
   |Region|(USA) USA, Osten|
   |Image|**Windows Server 2019 Datacenter, Gen 2**|
   |Size|**Standard D2s v3**|
   |Username|**Kursteilnehmer**|
   |Kennwort|**Pa55w.rd1234**|
   |Öffentliche Eingangsports|**None**|
   |Sie verfügen bereits über eine Windows Server-Lizenz|**Nein**|

    >**Hinweis**: Für öffentliche eingehende Ports verwenden wir die vorab erstellte Netzwerksicherheitsgruppe. 

1. Klicken Sie auf **Weiter: Datenträger >** , und legen Sie auf dem Blatt **Virtuellen Computer erstellen** auf der Registerkarte **Datenträger** den **Typ des Betriebssystemdatenträgers** auf **HDD Standard** fest, und klicken Sie dann auf **Weiter: Netzwerk >** .

1. Wählen Sie auf der Registerkarte **Netzwerk** des Blatts **Virtuellen Computer erstellen** das zuvor erstellte Netzwerk **myVirtualNetwork** aus.

1. Wählen Sie unter **NIC-Netzwerksicherheitsgruppe** die Option **Keine** aus.

1. Klicken Sie auf **Weiter: Verwaltung >** , und geben Sie auf der Registerkarte **Verwaltung** des Blatts **Virtuellen Computer erstellen** die folgenden Einstellungen ein.

   |Einstellung|Wert|
   |---|---|
   |Startdiagnose|**Mit verwaltetem Speicherkonto aktiviert (empfohlen)**|

1. Klicken Sie auf **Überprüfen + erstellen**. Vergewissern Sie sich auf dem Blatt **Überprüfen + erstellen**, dass die Überprüfung erfolgreich war, und klicken Sie auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis beide virtuellen Computer bereitgestellt wurden, bevor Sie fortfahren. 

#### <a name="task-3-associate-each-virtual-machines-network-interface-to-its-application-security-group"></a>Aufgabe 3: Zuordnen der einzelnen Netzwerkschnittstellen virtueller Computer zur entsprechenden Anwendungssicherheitsgruppe.

In dieser Aufgabe ordnen Sie die einzelnen Netzwerkschnittstellen virtueller Computer zur entsprechenden Anwendungssicherheitsgruppe zu. Die Schnittstelle des virtuellen Computers „myVMWeb“ wird der ASG „myAsgWebServers“ zugeordnet. Die Schnittstelle des virtuellen Computers „myVMMgmt“ wird der ASG „myAsgMgmtServers“ zugeordnet. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Virtuelle Computer**, und vergewissern Sie sich, dass die beiden virtuellen Computer mit dem Status **Wird ausgeführt** aufgeführt sind.

1. Klicken Sie in der Liste der virtuellen Computer auf den Eintrag **myVMWeb**.

1. Klicken Sie auf dem Blatt **myVMWeb** im Abschnitt **Einstellungen** auf **Netzwerk** und dann auf dem Blatt **myVMWeb \| Netzwerk** auf die Registerkarte **Anwendungssicherheitsgruppen**.

1. Klicken Sie auf **Anwendungssicherheitsgruppen konfigurieren**, wählen Sie in der Dropdownliste **Anwendungssicherheitsgruppe** die Option **myAsgWebServers** aus, und klicken Sie dann auf **Speichern**.

1. Navigieren Sie zurück zum Blatt **Virtuelle Computer**, und klicken Sie in der Liste der virtuellen Computer auf den Eintrag **myVMMgmt**.

1. Klicken Sie auf dem Blatt **myVMMgmt** im Abschnitt **Einstellungen** auf **Netzwerk** und dann auf dem Blatt **myVMMgmt\| Netzwerk** auf die Registerkarte **Anwendungssicherheitsgruppen**.

1. Klicken Sie auf **Anwendungssicherheitsgruppen konfigurieren**, wählen Sie in der Dropdownliste **Anwendungssicherheitsgruppe** die Option **myAsgMgmtServers** aus, und klicken Sie dann auf **Speichern**.

#### <a name="task-4-test-the-network-traffic-filtering"></a>Aufgabe 4: Testen der Filterung des Netzwerkdatenverkehrs

In dieser Aufgabe testen Sie die Filter für den Netzwerkdatenverkehr. Sie sollten in der Lage sein, RDP in den virtuellen Computer „myVMMgmt“ zu integrieren. Sie sollten über das Internet eine Verbindung mit dem virtuellen Computer „myVMWeb“ herstellen und die IIS-Standardwebseite anzeigen können.  

1. Navigieren Sie zurück zum Blatt **myVMMgmt** des virtuellen Computers.

1. Klicken Sie auf dem Blatt **myVMMgmt** auf **Verbinden** und dann im Dropdownmenü auf **RDP**. 

1. Klicken Sie auf **RDP-Datei herunterladen**, und stellen Sie damit eine Verbindung mit der Azure-VM **myVMMgmt** über Remotedesktop her. Wenn Sie zur Authentifizierung aufgefordert werden, geben Sie die folgenden Anmeldeinformationen an:

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**Kursteilnehmer**|
   |Kennwort|**Pa55w.rd1234**|

    >**Hinweis**: Stellen Sie sicher, dass die Remotedesktopverbindung erfolgreich hergestellt wurde. An diesem Punkt haben Sie sich vergewissert, dass Sie eine Verbindung zu „myVMMgmt“ über Remotedesktop herstellen können.

1. Navigieren Sie im Azure-Portal zum Blatt **myVMWeb** des virtuellen Computers.

1. Klicken Sie auf dem Blatt **myVMWeb** im Abschnitt **Vorgänge** auf **Skriptausführung**, und klicken Sie dann auf **RunPowerShellScript**.

1. Führen Sie im Bereich **Befehlsskript ausführen** Folgendes aus, um die Webserverrolle auf **myVmWeb** zu installieren:

    ```powershell
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    ```

    >**Hinweis**: Warten Sie, bis die Installation abgeschlossen ist. Dieser Vorgang kann einige Minuten in Anspruch nehmen. An diesem Punkt können Sie überprüfen, ob auf „myVMWeb“ über HTTP/HTTPS zugegriffen werden kann.

1. Navigieren Sie im Azure-Portal zurück zum Blatt **myVMWeb**.

1. Suchen Sie auf dem Blatt **myVMWeb** die **öffentliche IP-Adresse** des virtuellen Azure-Computers „myVmWeb“.

1. Öffnen Sie eine weitere Browserregisterkarte, und navigieren Sie zu der IP-Adresse, die Sie im vorherigen Schritt identifiziert haben.

    >**Hinweis**: Auf der Browserseite sollte die standardmäßige IIS-Willkommensseite angezeigt werden, da Port 80 basierend auf der Einstellung der Anwendungssicherheitsgruppe **myAsgWebServers** für eingehenden Datenverkehr aus dem Internet zulässig ist. Die Netzwerkschnittstelle des virtuellen Azure-Computers „myVMWeb“ ist dieser Anwendungssicherheitsgruppe zugeordnet. 

> Ergebnis: Sie haben überprüft, ob die NSG- und ASG-Konfiguration funktioniert und der Datenverkehr ordnungsgemäß verwaltet wird. 

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen. 

1. Öffnen Sie den Cloud Shell-Bereich, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Wenn Sie dazu aufgefordert werden, wählen Sie **PowerShell** und dann **Speicher erstellen** aus.

1. Stellen Sie sicher, dass im Dropdownmenü oben links im Cloud Shell-Bereich der Eintrag **PowerShell** ausgewählt ist.

1. Führen Sie im Cloud Shell-Bereich in der PowerShell-Sitzung den folgenden Code aus, um die Ressourcengruppe zu entfernen, die Sie in diesem Lab erstellt haben:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB07" -Force -AsJob
    ```

1.  Schließen Sie den **Cloud Shell**-Bereich. 
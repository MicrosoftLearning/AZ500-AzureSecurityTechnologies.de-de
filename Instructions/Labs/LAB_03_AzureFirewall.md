---
lab:
  title: 03 – Azure Firewall
  module: Module 02 - Plan and implement security for public access to Azure resources
---

# Lab 03: Azure Firewall
# Lab-Handbuch für Kursteilnehmer

## Labszenario

Sie wurden aufgefordert, Azure Firewall zu installieren. Damit möchte Ihre Organisation den ein- und ausgehenden Netzwerkzugriff steuern. Dies ist ein wichtiger Bestandteil eines allgemeinen Netzwerksicherheitsplans. Insbesondere sollen Sie die folgenden Infrastrukturkomponenten erstellen und testen:

- Ein virtuelles Netzwerk mit einem Workloadsubnetz und einem Bastion Host-Subnetz.
- Eine VM in jedem Subnetz. 
- Eine benutzerdefinierte Route, die sicherstellt, dass der gesamte ausgehende Workloaddatenverkehr aus dem Workloadsubnetz die Firewall durchlaufen muss.
- Firewallanwendungsregeln, die nur ausgehenden Datenverkehr an „www.bing.com“ zulassen. 
- Firewallnetzwerkregeln, die die externe DNS-Serversuche zulassen.

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Bereitstellen und Testen einer Azure Firewall-Instanz

## Azure Firewall-Diagramm

![image](https://user-images.githubusercontent.com/91347931/157529954-a1bc434b-2eca-41c1-b875-1f0c977d5e20.png)

## Anweisungen

## Lab-Dateien:

- **\\Allfiles\\Labs\\08\\template.json**

### Übung 1: Bereitstellen und Testen einer Azure Firewall-Instanz

### Geschätzte Zeit: 40 Minuten

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Verwenden einer Vorlage zum Bereitstellen der Labumgebung. 
- Aufgabe 2: Bereitstellen einer Azure Firewall-Instanz.
- Aufgabe 3: Erstellen einer Standardroute.
- Aufgabe 4: Konfigurieren einer Anwendungsregel.
- Aufgabe 5: Konfigurieren einer Netzwerkregel. 
- Aufgabe 6: Konfigurieren von DNS-Servern.
- Aufgabe 7: Testen der Firewall. 

#### Aufgabe 1: Verwenden einer Vorlage zum Bereitstellen der Labumgebung. 

In dieser Aufgabe überprüfen Sie die Labumgebung und stellen sie bereit. 

In dieser Aufgabe erstellen Sie eine VM mithilfe einer ARM-Vorlage. Diese VM wird in der letzten Übung für dieses Lab verwendet. 

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, dem in dem Azure-Abonnement, das Sie für dieses Lab verwenden, die Rolle „Besitzer“ oder „Mitwirkender“ zugewiesen ist.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Text **Benutzerdefinierte Vorlage bereitstellen** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** auf die Option **Eigene Vorlage im Editor erstellen**.

4. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Datei laden**, navigieren Sie zu **\\Allfiles\\Labs\\08\\template.json**, und klicken Sie auf **Öffnen**.

    >**Hinweis**: Überprüfen Sie den Inhalt der Vorlage, und beachten Sie, dass sie eine Azure-VM bereitstellt, die Windows Server 2016 Datacenter hostet.

5. Klicken Sie auf dem Blatt **Vorlage bearbeiten** auf **Speichern**.

6. Stellen Sie auf dem Blatt **Benutzerdefinierte Bereitstellung** sicher, dass die folgenden Einstellungen konfiguriert sind (übernehmen Sie für alle anderen Einstellungen die Standardwerte):

   |Einstellung|Wert|
   |---|---|
   |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
   |Resource group|Klicken Sie auf **Neu erstellen**, und geben Sie den Namen **AZ500LAB08** ein.|
   |Standort|**(USA) USA, Osten**|

    >**Hinweis**: Informationen zum Identifizieren von Azure-Regionen, in denen Sie Azure-VMs bereitstellen können, finden Sie unter [ **https://azure.microsoft.com/en-us/regions/offers/** ](https://azure.microsoft.com/en-us/regions/offers/).

7. Klicken Sie auf **Überprüfen und erstellen** und dann auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. Dieser Vorgang dauert etwa zwei Minuten. 

#### Aufgabe 2: Bereitstellen der Azure-Firewall

In dieser Aufgabe stellen Sie die Azure-Firewall im virtuellen Netzwerk bereit. 

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Firewalls** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **Firewalls** auf **+ Erstellen**.

3. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Firewall erstellen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

   |Einstellung|Wert|
   |---|---|
   |Resource group|**AZ500LAB08**|
   |Name|**Test-FW01**|
   |Region|**(USA) USA, Osten**|
   |Firewall-SKU|**Standard**|
   |Firewallverwaltung|**Use Firewall rules (classic) to manage this firewall** (Firewallregeln (klassisch) zum Verwalten dieser Firewall verwenden)|
   |Virtuelles Netzwerk auswählen|Klicken Sie auf die Option **Vorhandene verwenden**, und wählen Sie in der Dropdownliste **Test-FW-VN** aus.|
   |Öffentliche IP-Adresse|Klicken Sie auf **Neue hinzufügen**, geben Sie den Namen **TEST-FW-PIP** ein, und klicken Sie auf **OK**.|

4. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**. 

    >**Hinweis**: Warten Sie, bis die Bereitstellung abgeschlossen ist. Dieser Vorgang dauert etwa fünf Minuten. 

5. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Ressourcengruppen** ein, und drücken Sie die **EINGABETASTE**.

6. Klicken Sie auf dem Blatt **Ressourcengruppen** in der Liste der Ressourcengruppen auf den Eintrag **AZ500LAB08**.

    >**Hinweis**: Überprüfen Sie auf dem Blatt mit der Ressourcengruppe **AZ500LAB08** die Liste der Ressourcen. Sie können nach **Typ** sortieren.

7. Klicken Sie in der Liste der Ressourcen auf den Eintrag, der die Firewall **Test-FW01** darstellt.

8. Identifizieren Sie auf dem Blatt **Test-FW01** die **private IP-Adresse**, die der Firewall zugewiesen wurde. 

    >**Hinweis**: Sie benötigen diese Informationen für die nächste Aufgabe.


#### Aufgabe 3: Erstellen einer Standardroute

In dieser Aufgabe erstellen Sie eine Standardroute für das Subnetz **Workload-SN**. Mit dieser Route wird ausgehender Datenverkehr über die Firewall konfiguriert.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Routingtabellen** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **Routingtabellen** auf **+ Erstellen**.

3. Legen Sie auf dem Blatt **Neue Routingtabelle erstellen** folgende Einstellungen fest:

   |Einstellung|Wert|
   |---|---|
   |Resource group|**AZ500LAB08**|
   |Region| **USA, Osten**|
   |Name|**Firewall-route**|

4. Klicken Sie auf **Überprüfen + erstellen** und anschließend auf **Erstellen**. Warten Sie dann, bis die Bereitstellung abgeschlossen ist. 

5. Klicken Sie auf dem Blatt **Routingtabellen** auf **Aktualisieren**, und klicken Sie in der Liste der Routingtabellen auf den Eintrag **Firewall-route**.

6. Klicken Sie auf dem Blatt **Firewall-route** im Abschnitt **Einstellungen** auf **Subnetze**, und klicken Sie dann auf dem Blatt **Firewall-route \| Subnetze** auf **+ Zuordnen**.

7. Geben Sie auf dem Blatt **Subnetz zuordnen** die folgenden Einstellungen an:

   |Einstellung|Wert|
   |---|---|
   |Virtuelles Netzwerk|**Test-FW-VN**|
   |Subnet|**Workload-SN**|

    >**Hinweis**: Stellen Sie sicher, dass für diese Route nur das Subnetz **Workload-SN** ausgewählt ist. Andernfalls funktioniert die Firewall nicht korrekt.

8. Klicken Sie auf **OK**, um die Firewall dem Subnetz des virtuellen Netzwerks zuzuordnen. 

9. Klicken Sie zurück auf dem Blatt **Firewall-route** im Abschnitt **Einstellungen** auf **Routen** und dann auf **+ Hinzufügen**. 

10. Geben Sie auf dem Blatt **Route hinzufügen** die folgenden Einstellungen an:  

   |Einstellung|Wert|
   |---|---|
   |Routenname|**FW-DG**|
   |Adresspräfix für das Ziel|**IP-Adresse**|
   |Ziel-IP-Adressen/CIDR-Bereiche|**0.0.0.0/0**
   |Typ des nächsten Hops|**Virtuelles Gerät**|
   |Adresse des nächsten Hops|die private IP-Adresse der Firewall, die Sie in der vorherigen Aufgabe identifiziert haben|

    >**Hinweis**: Azure Firewall ist eigentlich ein verwalteter Dienst, in dieser Situation kann aber „Virtuelles Gerät“ verwendet werden.
    
11.  Klicken Sie auf **Hinzufügen**, um die Route hinzuzufügen. 


#### Aufgabe 4: Konfigurieren einer Anwendungsregel

In dieser Aufgabe erstellen Sie eine Anwendungsregel, die ausgehenden Zugriff auf `www.bing.com` ermöglicht.

1. Navigieren Sie im Azure-Portal zurück zur Firewall **Test-FW01**.

2. Klicken Sie auf dem Blatt **Test-FW01** im Abschnitt **Einstellungen** auf **Regeln (klassisch)** .

3. Klicken Sie auf dem Blatt **Test-FW01 \|Regeln (klassisch)** auf die Registerkarte **Anwendungsregelsammlung** und dann auf **+ Anwendungsregelsammlung hinzufügen**.

4. Geben Sie auf dem Blatt **Anwendungsregelsammlung hinzufügen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

   |Einstellung|Wert|
   |---|---|
   |Name|**App-Coll01**|
   |Priorität|**200**|
   |Aktion|**Zulassen**|

5. Erstellen Sie auf dem Blatt **Anwendungsregelsammlung hinzufügen** einen neuen Eintrag im Abschnitt **Ziel-FQDNs** mit den folgenden Einstellungen (übernehmen Sie die Standardwerte für andere Einstellungen):

   |Einstellung|Wert|
   |---|---|
   |name|**AllowGH**|
   |Quelltyp|**IP-Adresse**|
   |`Source`|**10.0.2.0/24**|
   |Protokollport|**http:80, https:443**|
   |Ziel-FQDNs|**www.bing.com**|

6. Klicken Sie auf **Hinzufügen**, um die auf Ziel-FQDNs basierende Anwendungsregel hinzuzufügen.

    >**Hinweis**: Azure Firewall enthält eine integrierte Regelsammlung für Infrastruktur-FQDNs, die standardmäßig zulässig sind. Diese FQDNs sind plattformspezifisch und können nicht für andere Zwecke verwendet werden. 

#### Aufgabe 5: Konfigurieren einer Netzwerkregel

In dieser Aufgabe erstellen Sie eine Netzwerkregel, die ausgehenden Zugriff auf zwei IP-Adressen an Port 53 (DNS) zulässt.

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Test-FW01 \|Regeln (klassisch)** .

2. Klicken Sie auf dem Blatt **Test-FW01 \|Regeln (klassisch)** auf die Registerkarte **Netzwerkregelsammlung** und dann auf **+ Netzwerkregelsammlung hinzufügen**.

3. Geben Sie auf dem Blatt **Netzwerkregelsammlung hinzufügen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

   |Einstellung|Wert|
   |---|---|
   |Name|**Net-Coll01**|
   |Priorität|**200**|
   |Aktion|**Zulassen**|

4. Erstellen Sie auf dem Blatt **Netzwerkregelsammlung hinzufügen** einen neuen Eintrag im Abschnitt **IP-Adressen** mit den folgenden Einstellungen (übernehmen Sie die Standardwerte für andere Einstellungen):

   |Einstellung|Wert|
   |---|---|
   |Name|**AllowDNS**|
   |Protocol|**UDP**|
   |Quellentyp|**IP-Adresse**|
   |Quelladressen|**10.0.2.0/24**|
   |Zieltyp|**IP-Adresse**|
   |Destination Address (Zieladresse)|**209.244.0.3,209.244.0.4**|
   |Zielports|**53**|

5. Klicken Sie auf **Hinzufügen**, um die Netzwerkregel hinzuzufügen.

    >**Hinweis**: Die in diesem Fall verwendeten Zieladressen sind bekannte öffentliche DNS-Server. 

#### Aufgabe 6: Konfigurieren der DNS-Server für VMs

In dieser Aufgabe konfigurieren Sie die primären und sekundären DNS-Adressen für den virtuellen Computer. Dies ist keine Firewallanforderung. 

1. Navigieren Sie im Azure-Portal zur Ressourcengruppe **AZ500LAB08**.

2. Klicken Sie auf dem Blatt **AZ500LAB08** in der Liste der Ressourcen auf den virtuellen Computer **Srv-Work**.

3. Klicken Sie auf dem Blatt **Srv-Work** im Abschnitt **Einstellungen** auf **Netzwerk**.

4. Klicken Sie auf dem Blatt **Srv-Work \| Netzwerk** auf den Link neben dem Eintrag **Netzwerkschnittstelle**.

5. Klicken Sie auf dem Blatt „Netzwerkschnittstelle“ im Abschnitt **Einstellungen** auf **DNS-Server**, wählen Sie die Option **Benutzerdefiniert** aus, fügen Sie die beiden DNS-Server hinzu, auf die in der Netzwerkregel **209.244.0.3** und **209.244.0.4** verwiesen wird, und klicken Sie auf **Speichern**, um die Änderung zu speichern.

6. Kehren Sie zur VM-Seite **Srv-Work** zurück.

    >**Hinweis**: Warten Sie, bis die Aktualisierung abgeschlossen wurde.

    >**Hinweis**: Durch Aktualisieren der DNS-Server für eine Netzwerkschnittstelle wird der virtuelle Computer, an den die Schnittstelle angefügt ist, automatisch neu gestartet. Gegebenenfalls werden auch alle anderen virtuellen Computer in derselben Verfügbarkeitsgruppe neu gestartet.

#### Aufgabe 7: Testen der Firewall

In dieser Aufgabe testen Sie die Firewall, um sich zu vergewissern, dass sie wie erwartet funktioniert.

1. Navigieren Sie im Azure-Portal zur Ressourcengruppe **AZ500LAB08**.

2. Klicken Sie auf dem Blatt **AZ500LAB08** in der Liste der Ressourcen auf den virtuellen Computer **Srv-Jump**.

3. Klicken Sie auf dem Blatt **Srv-Jump** auf **Verbinden** und dann im Dropdownmenü auf **RDP**. 

4. Klicken Sie auf **RDP-Datei herunterladen**, und stellen Sie damit eine Verbindung mit der Azure-VM **Srv-Jump** über Remotedesktop her. Wenn Sie zur Authentifizierung aufgefordert werden, geben Sie die folgenden Anmeldeinformationen an:

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**localadmin**|
   |Kennwort|**Pa55w.rd1234**|

    >**Hinweis**: Die folgenden Schritte werden in der Remotedesktopsitzung mit der Azure-Computer-VM **Srv-Jump** ausgeführt. 

    >**Hinweis**: Sie stellen eine Verbindung mit dem virtuellen Computer **Srv-Work** her. Dies geschieht, damit wir testen können, ob der Zugriff auf die Website „bing.com“ möglich ist.  

5. Klicken Sie in der Remotedesktopsitzung mit **Srv-Jump** mit der rechten Maustaste auf **Start**, klicken Sie im Kontextmenü auf **Ausführen**, und führen Sie über das Dialogfeld **Ausführen** den folgenden Code aus, um eine Verbindung mit **Srv-Work** herzustellen. 

    ```
    mstsc /v:Srv-Work
    ```

6. Wenn Sie zur Authentifizierung aufgefordert werden, geben Sie die folgenden Anmeldeinformationen an:

   |Einstellung|Wert|
   |---|---|
   |Benutzername|**localadmin**|
   |Kennwort|**Pa55w.rd1234**|

    >**Hinweis**: Warten Sie, bis die Remotedesktopsitzung eingerichtet und die Server-Manager Schnittstelle geladen wurde.

7. Klicken Sie in der Remotedesktopsitzung mit **Srv-Jump** unter **Server-Manager** auf **Lokaler Server** und dann auf **Verstärkte Sicherheitskonfiguration für IE**.

8. Legen Sie im Dialogfeld **Verstärkte Sicherheitskonfiguration für Internet Explorer** beide Optionen auf **Aus** fest, und klicken Sie auf **OK**.

9. Starten Sie in der Remotedesktopsitzung mit **Srv-Jump** den Internet Explorer, und navigieren Sie zu **`https://www.bing.com`** . 

    >**Hinweis**: Die Website sollte erfolgreich angezeigt werden. Die Firewall ermöglicht Ihnen den Zugriff.

10. Rufen Sie **`http://www.microsoft.com/`** auf.

    >**Hinweis**: Auf der Browserseite sollte eine Meldung mit etwa folgendem Text angezeigt werden: `HTTP request from 10.0.2.4:xxxxx to microsoft.com:80. Action: Deny. No rule matched. Proceeding with default action.` Dies ist zu erwarten, da die Firewall den Zugriff auf diese Website blockiert. 

11. Beenden Sie beide Remotedesktopsitzungen.

> Ergebnis: Sie haben Azure Firewall erfolgreich konfiguriert und getestet.

**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen. 

1. Öffnen Sie im Azure-Portal Cloud Shell, indem Sie oben rechts im Azure-Portal auf das erste Symbol klicken. Klicken Sie bei der entsprechenden Aufforderung auf **PowerShell** und dann auf **Speicher erstellen**.

2. Stellen Sie sicher, dass oben links im Cloud Shell-Bereich im Dropdownmenü der Eintrag **PowerShell** ausgewählt ist.

3. Führen Sie im Cloud Shell-Bereich in der PowerShell-Sitzung den folgenden Code aus, um die Ressourcengruppe zu entfernen, die Sie in diesem Lab erstellt haben:
  
    ```powershell
    Remove-AzResourceGroup -Name "AZ500LAB08" -Force -AsJob
    ```
4. Schließen Sie den **Cloud Shell**-Bereich. 

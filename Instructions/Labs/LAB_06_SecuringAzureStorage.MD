---
lab:
  title: ' 06 – Dienstendpunkte und Schützen von Speicher'
  module: Module 03 - Plan and implement security for Azure SQL Database and Azure SQL Managed Instance
---

# Lab 06: Dienstendpunkte und Schützen von Speicher
# Lab-Handbuch für Kursteilnehmer

## Labszenario

Sie sollen eine Machbarkeitsstudie (Proof of Concept) erstellen, um das Schützen von Azure-Dateifreigaben zu veranschaulichen. Insbesondere geht es Ihnen um Folgendes:
    
- Sie möchten einen Speicherendpunkt erstellen, damit der für Azure Storage bestimmte Datenverkehr immer innerhalb des Azure-Backbonenetzwerks bleibt.
- Sie möchten den Speicherendpunkt so konfigurieren, dass nur Ressourcen aus einem bestimmten Subnetz auf den Speicher zugreifen können.
- Sie möchten sicherstellen, dass Ressourcen außerhalb des bestimmten Subnetzes nicht auf den Speicher zugreifen können. 

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Dienstendpunkte und Sicherheitsspeicher

## Diagramm zu Dienstendpunkten und Schützen von Speicher

![image](https://user-images.githubusercontent.com/91347931/157534883-29664a05-85d1-4c70-99a7-f16d2360755d.png)

## Anweisungen

### Übung 1: Dienstendpunkte und Sicherheitsspeicher

### Geschätzte Dauer: 45 Minuten

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Erstellen eines virtuellen Netzwerks
- Aufgabe 2: Hinzufügen eines Subnetzes zum virtuellen Netzwerk und Konfigurieren eines Speicherendpunkts
- Aufgabe 3: Konfigurieren einer Netzwerksicherheitsgruppe zum Einschränken des Zugriffs auf das Subnetz
- Aufgabe 4: Konfigurieren einer Netzwerksicherheitsgruppe zum Zulassen von RDP im öffentlichen Subnetz
- Aufgabe 5: Erstellen eines Speicherkontos mit einer Dateifreigabe
- Aufgabe 6: Bereitstellen virtueller Computer in den angegebenen Subnetzen
- Aufgabe 7: Testen der Speicherverbindung vom privaten Subnetz, um sicherzustellen, dass der Zugriff zulässig ist
- Aufgabe 8: Testen der Speicherverbindung vom öffentlichen Subnetz, um sicherzustellen, dass der Zugriff verweigert wird 

#### Aufgabe 1: Erstellen eines virtuellen Netzwerks

In dieser Aufgabe erstellen Sie ein virtuelles Netzwerk.

1. Melden Sie sich beim Azure-Portal ( **`https://portal.azure.com/`** ) an.

    >**Hinweis**: Melden Sie sich beim Azure-Portal mit einem Konto an, dem in dem Azure-Abonnement, das Sie für dieses Lab verwenden, die Rolle „Besitzer“ oder „Mitwirkender“ zugewiesen ist.

2. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Virtuelle Netzwerke** ein, und drücken Sie die **EINGABETASTE**.

3. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** auf  **+ Erstellen**.

4. Geben Sie auf dem Blatt **Virtuelles Netzwerk erstellen** auf der Registerkarte **Grundeinstellungen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen), und klicken Sie auf **Weiter: IP-Adressen**:

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
    |Resource group|Klicken Sie auf **Neu erstellen**, und geben Sie den Namen **AZ500LAB12** ein.|
    |Name|**myVirtualNetwork**|
    |Region|**(USA) USA, Osten**|

5. Legen Sie auf dem Blatt **Virtuelles Netzwerk erstellen** auf der Registerkarte **IP-Adressen** den **IPv4-Adressraum** auf **10.0.0.0/16** fest. Klicken Sie in der Spalte **Subnetzname** auf **Standard**, und geben Sie auf dem Blatt **Subnetz bearbeiten** die folgenden Einstellungen an, und klicken Sie dann auf **Speichern**:

    |Einstellung|Wert|
    |---|---|
    |Subnetzname|**Public**|
    |Subnetzadressbereich|**10.0.0.0/24**|

6. Klicken Sie anschließend auf dem Blatt **Virtuelles Netzwerk erstellen** auf der Registerkarte **IP-Adressen** auf **Überprüfen und erstellen**.

7. Klicken Sie auf dem Blatt **Virtuelles Netzwerk erstellen** auf der Registerkarte **Überprüfen und erstellen** auf **Erstellen**.

#### Aufgabe 2: Hinzufügen eines Subnetzes zum virtuellen Netzwerk und Konfigurieren eines Speicherendpunkts

In dieser Aufgabe erstellen Sie ein weiteres Subnetz und aktivieren einen Dienstendpunkt in diesem Subnetz. Dienstendpunkte werden pro Dienst und pro Subnetz aktiviert. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Virtuelle Netzwerke**.

2. Klicken Sie auf dem Blatt **Virtuelle Netzwerke** auf den Eintrag **myVirtualNetwork**.

3. Klicken Sie auf dem Blatt **myVirtualNetwork** im Abschnitt **Einstellungen** auf **Subnetze**.

4. Klicken Sie auf dem Blatt **myVirtualNetwork \| Subnetze** auf **+ Subnetz**. 

5. Geben Sie auf dem Blatt **Subnetz hinzufügen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    |Einstellung|Wert|
    |---|---|
    |Subnetzname|**Privat**|
    |Subnetzadressbereich|**10.0.1.0/24**|
    |Dienstendpunkte|**Übernehmen Sie den Standardwert „Keine“.**|

6. Klicken Sie auf dem Blatt **Subnetz hinzufügen** auf **Speichern**, um das neue Subnetz hinzuzufügen.

    >**Hinweis**: Das virtuelle Netzwerk umfasst jetzt zwei Subnetze: ein öffentliches und ein privates Subnetz. 
    
#### Aufgabe 3: Konfigurieren einer Netzwerksicherheitsgruppe zum Einschränken des Zugriffs auf das Subnetz

In dieser Aufgabe erstellen Sie eine Netzwerksicherheitsgruppe mit zwei Sicherheitsregeln für ausgehenden Datenverkehr (Azure Storage und Internet) und einer Sicherheitsregel für eingehenden Datenverkehr (Remotedesktopprotokoll, RDP). Außerdem ordnen Sie die Netzwerksicherheitsgruppe dem privaten Subnetz zu. Dadurch wird der ausgehende Datenverkehr von den mit diesem Subnetz verbundenen Azure-VMs eingeschränkt.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Netzwerksicherheitsgruppen** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **Netzwerksicherheitsgruppen** auf **+ Erstellen**.

3. Geben Sie auf dem Blatt **Netzwerksicherheitsgruppe erstellen** auf der Registerkarte **Grundeinstellungen** die folgenden Einstellungen an: 

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
    |Resource group|**AZ500LAB12**|
    |Name|**myNsgPrivate**|
    |Region|**USA, Osten**|

4. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis**: In den nächsten Schritten erstellen Sie eine Sicherheitsregel für ausgehenden Datenverkehr, die die Kommunikation mit dem Azure Storage-Dienst zulässt. 

5. Navigieren Sie im Azure-Portal zurück zum Blatt **Netzwerksicherheitsgruppen**, und klicken Sie auf den Eintrag **myNsgPrivate**.

6. Klicken Sie auf dem Blatt **myNsgPrivate** im Abschnitt **Einstellungen** auf **Sicherheitsregeln für ausgehenden Datenverkehr**.

7. Klicken Sie auf dem Blatt **myNsgPrivate \| Sicherheitsregeln für ausgehenden Datenverkehr** auf **+ Hinzufügen**.

8. Geben Sie auf dem Blatt **Sicherheitsregel für ausgehenden Datenverkehr hinzufügen** die folgenden Einstellungen an, um ausgehenden Datenverkehr an Azure Storage explizit zuzulassen (übernehmen Sie für alle anderen Werte die Standardeinstellungen): 

    |Einstellung|Wert|
    |---|---|
    |`Source`|**Diensttag**|
    |Quelldiensttag|**VirtualNetwork**|
    |Source port ranges|**\***|
    |Destination|**Diensttag**|
    |Zieldiensttag|**Storage**|
    |Zielportbereiche|**\***|
    |Protocol|**Alle**|
    |Aktion|**Zulassen**|
    |Priorität|**1000**|
    |Name|**Allow-Storage-All**|

9. Klicken Sie auf dem Blatt **Sicherheitsregel für ausgehenden Datenverkehr hinzufügen** auf **Hinzufügen**, um die neue Regel für ausgehenden Datenverkehr zu erstellen. 

10. Klicken Sie auf dem Blatt **myNsgPrivate** im Abschnitt **Einstellungen** auf **Sicherheitsregeln für ausgehenden Datenverkehr**, und klicken Sie dann auf **+ Hinzufügen**.

11. Geben Sie auf dem Blatt **Sicherheitsregel für ausgehenden Datenverkehr hinzufügen** die folgenden Einstellungen an, um ausgehenden Datenverkehr an das Internet explizit zu verweigern (übernehmen Sie für alle anderen Werte die Standardeinstellungen): 

    |Einstellung|Wert|
    |---|---|
    |`Source`|**Diensttag**|
    |Quelldiensttag|**VirtualNetwork**|
    |Source port ranges|**\***|
    |Destination|**Diensttag**|
    |Zieldiensttag|**Internet**|
    |Zielportbereiche|**\***|
    |Protocol|**Alle**|
    |Aktion|**Deny** (Verweigern)|
    |Priority|**1100**|
    |Name|**Deny-Internet-All**|

    >**Hinweis**: Diese Regel setzt eine Standardregel in allen Netzwerksicherheitsgruppen außer Kraft, die ausgehende Internetkommunikation zulässt. 

    >**Hinweis**: In den nächsten Schritten erstellen Sie eine Sicherheitsregel für eingehenden Datenverkehr, die RDP-Datenverkehr an das Subnetz zulässt. Die Regel setzt eine Standardsicherheitsregel außer Kraft, die jeglichen eingehenden Verkehr aus dem Internet abweist. Remotedesktopverbindungen mit dem Subnetz sind zulässig, sodass die Konnektivität in einem späteren Schritt getestet werden kann.

12. Klicken Sie auf dem Blatt **myNsgPrivate** im Abschnitt **Einstellungen** auf **Sicherheitsregeln für eingehenden Datenverkehr**, und klicken Sie dann auf **+ Hinzufügen**.

13. Geben Sie auf dem Blatt **Sicherheitsregel für eingehenden Datenverkehr hinzufügen** die folgenden Einstellungen an (übernehmen Sie für alle anderen Einstellungen die Standardwerte): 

    |Einstellung|Wert|
    |---|---|
    |Quelle|**Alle**|
    |Quellportbereiche|**\***|
    |Destination|**Diensttag**|
    |Zieldiensttag|**VirtualNetwork**|
    |Zielportbereiche|**3389**|
    |Protokoll|**TCP**|
    |Aktion|**Zulassen**|
    |Priorität|**1200**|                                                    
    |Name|**Allow-RDP-All**|

14. Klicken Sie auf dem Blatt **Sicherheitsregel für eingehenden Datenverkehr hinzufügen** auf **Hinzufügen**, um die neue Regel für eingehenden Datenverkehr zu erstellen. 

    >**Hinweis**: Jetzt ordnen Sie die Netzwerksicherheitsgruppe dem privaten Subnetz zu.

15. Wählen Sie auf dem Blatt **Subnetze** die Option **+ Zuordnen** aus, geben Sie im Abschnitt **Subnetz zuordnen** die folgenden Einstellungen an, und klicken Sie dann auf **OK**:

    |Einstellung|Wert|
    |---|---|
    |Virtuelles Netzwerk|**myVirtualNetwork**|
    |Subnet|**Privat**|

#### Aufgabe 4: Konfigurieren einer Netzwerksicherheitsgruppe zum Zulassen von RDP im öffentlichen Subnetz

In dieser Aufgabe erstellen Sie eine Netzwerksicherheitsgruppe mit einer Sicherheitsregel für eingehenden Datenverkehr (RDP). Außerdem ordnen Sie die Netzwerksicherheitsgruppe dem öffentlichen Subnetz zu. Dies ermöglicht den RDP-Zugriff auf den öffentlichen virtuellen Computer.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Netzwerksicherheitsgruppen** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **Netzwerksicherheitsgruppen** auf **+ Erstellen**.

3. Geben Sie auf dem Blatt **Netzwerksicherheitsgruppe erstellen** auf der Registerkarte **Grundeinstellungen** die folgenden Einstellungen an: 

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
    |Resource group|**AZ500LAB12**|
    |Name|**myNsgPublic**|
    |Region|**USA, Osten**|

4. Klicken Sie auf **Überprüfen + erstellen** und dann auf **Erstellen**.

    >**Hinweis**: In den nächsten Schritten erstellen Sie eine Sicherheitsregel für ausgehenden Datenverkehr, die die Kommunikation mit dem Azure Storage-Dienst zulässt. 

5. Navigieren Sie im Azure-Portal zurück zum Blatt **Netzwerksicherheitsgruppen**, und klicken Sie auf den Eintrag **myNsgPublic**.

6. Klicken Sie auf dem Blatt **myNsgPublic** im Abschnitt **Einstellungen** auf **Sicherheitsregeln für eingehenden Datenverkehr**, und klicken Sie dann auf **+ Hinzufügen**.

7. Geben Sie auf dem Blatt **Sicherheitsregel für eingehenden Datenverkehr hinzufügen** die folgenden Einstellungen an (übernehmen Sie für alle anderen Einstellungen die Standardwerte): 

    |Einstellung|Wert|
    |---|---|
    |Quelle|**Alle**|
    |Quellportbereiche|**\***|
    |Destination|**Diensttag**|
    |Zieldiensttag|**VirtualNetwork**|
    |Zielportbereiche|**3389**|
    |Protokoll|**TCP**|
    |Aktion|**Zulassen**|
    |Priorität|**1200**|                                                    
    |Name|**Allow-RDP-All**|

8. Klicken Sie auf dem Blatt **Sicherheitsregel für eingehenden Datenverkehr hinzufügen** auf **Hinzufügen**, um die neue Regel für eingehenden Datenverkehr zu erstellen. 

    >**Hinweis**: Jetzt ordnen Sie die Netzwerksicherheitsgruppe dem öffentlichen Subnetz zu.

9. Wählen Sie auf dem Blatt **Subnetze** die Option **+ Zuordnen** aus, geben Sie im Abschnitt **Subnetz zuordnen** die folgenden Einstellungen an, und klicken Sie dann auf **OK**:

    |Einstellung|Wert|
    |---|---|
    |Virtuelles Netzwerk|**myVirtualNetwork**|
    |Subnet|**Public**|
    
#### Aufgabe 5: Erstellen eines Speicherkontos mit einer Dateifreigabe

In dieser Aufgabe erstellen Sie ein Speicherkonto mit einer Dateifreigabe und rufen den Speicherkontoschlüssel ab.  

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Speicherkonten** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **Speicherkonten** auf  **+ Erstellen**.

3. Geben Sie auf der Registerkarte **Grundeinstellungen** des Blatts **Speicherkonto erstellen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
    |Resource group|**AZ500LAB12**|
    |Speicherkontoname|Ein beliebiger global eindeutiger Name, der zwischen 3 und 24 Zeichen lang ist und aus Buchstaben und Ziffern besteht.|
    |Location|**(USA) USA, Osten**|
    |Leistung|**Standard (allgemeines v2-Konto)**|
    |Redundanz|**Lokal redundanter Speicher (LRS)**|

4. Klicken Sie auf dem Blatt **Speicherkonto erstellen** auf der Registerkarte **Grundeinstellungen** auf **Überprüfen und erstellen**, warten Sie, bis der Überprüfungsprozess abgeschlossen ist, und klicken Sie dann auf **Erstellen**.

    >**Hinweis**: Warten Sie, bis das Speicherkonto erstellt wurde. Dieser Vorgang dauert etwa zwei Minuten.

5. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Ressourcengruppen** ein, und drücken Sie die **EINGABETASTE**.

6. Klicken Sie auf dem Blatt **Ressourcengruppen** in der Liste der Ressourcengruppen auf den Eintrag **AZ500LAB12**.

7. Klicken Sie auf dem Blatt für die Ressourcengruppe **AZ500LAB12** in der Ressourcenliste auf den Eintrag, der das neu erstellte Speicherkonto darstellt.

8. Klicken Sie auf dem Blatt **Übersicht** des Speicherkontos auf der Registerkarte **Datenspeicher** auf **Dateifreigaben**, und klicken Sie dann auf **+ Dateifreigabe**.

9. Deaktivieren Sie auf dem Blatt **Neue Dateifreigabe** in der Registerkarte Sicherung die Option **Sicherung aktivieren**. 

10. Geben Sie auf dem Blatt **Neue Dateifreigabe** die folgenden Einstellungen an:

    |Einstellung|Wert|
    |---|---|
    |Name|**my-file-share**|

11. Klicken Sie auf dem Blatt **Neue Dateifreigabe** auf **Erstellen**.

    >**Hinweis**: Rufen Sie jetzt das PowerShell-Skript ab, das eine Laufwerkzuordnung zur Azure-Dateifreigabe erstellt, und zeichnen Sie es auf. 

12. Klicken Sie auf dem Blatt des Speicherkontos in der Liste der Dateifreigaben auf **my-file-share**.

13. Klicken Sie auf dem Blatt **my-file-share** auf **Verbinden**.

14. Kopieren Sie auf dem Blatt **Verbinden** auf der Registerkarte **Windows** das PowerShell-Skript, das eine Zuordnung des Laufwerks „Z“ zur Dateifreigabe erstellt. 

    >**Hinweis**: Zeichnen Sie dieses Skript auf. Sie benötigen es später in diesem Lab, um die Dateifreigabe vom virtuellen Azure-Computer im **privaten** Subnetz zuzuordnen.

15. Navigieren Sie zurück zum Blatt des Speicherkontos, und klicken Sie dann im Abschnitt **Sicherheit und Netzwerk** auf **Netzwerk**.

16. Wählen Sie unter **Öffentlicher Netzwerkzugriff** die Option **Verwalten** aus und wählen Sie als **Standardaktion** die Option **Aus ausgewählten Netzwerken aktivieren** aus.

17. Wählen Sie auf dem Blatt **Ressourceneinstellungen: Virtuelle Netzwerke, IP-Adressen und Ausnahmen** die Option **Ansicht** aus, und klicken Sie auf den Link **+ Vorhandenes virtuelles Netzwerk hinzufügen**. 

18. Geben Sie auf dem Blatt **Netzwerke hinzufügen** die folgenden Einstellungen an:

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden|
    |Virtuelle Netzwerke|**myVirtualNetwork**|
    |Subnetze|**Privat**|

19. Klicken Sie auf dem Blatt **Netzwerke hinzufügen** auf **Hinzufügen**. 

20. Klicken Sie anschließend auf dem Blatt des Speicherkontos auf **Speichern**.

    >**Hinweis**: An diesem Punkt im Lab haben Sie ein virtuelles Netzwerk, eine Netzwerksicherheitsgruppe und ein Speicherkonto mit einer Dateifreigabe konfiguriert. 

#### Aufgabe 6: Bereitstellen virtueller Computer in den angegebenen Subnetzen

In dieser Aufgabe erstellen Sie zwei virtuelle Computer: einen im privaten Subnetz und einen im öffentlichen Subnetz. 

>**Hinweis**: Der erste virtuelle Computer wird mit dem privaten Subnetz verbunden.

1. Geben Sie im Azure-Portal oben auf der Azure-Portalseite im Textfeld **Nach Ressourcen, Diensten und Dokumenten suchen** den Begriff **Virtuelle Computer** ein, und drücken Sie die **EINGABETASTE**.

2. Klicken Sie auf dem Blatt **Virtuelle Computer** auf **+ Erstellen**, und klicken Sie in der Dropdownliste auf **+ Virtueller Azure-Computer**.

3. Geben Sie auf dem Blatt **Virtuellen Computer erstellen** auf der Registerkarte **Grundeinstellungen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
    |Resource group|**AZ500LAB12**|
    |Name des virtuellen Computers|**myVmPrivate**|
    |Region|**(USA) USA, Osten**|
    |Image|**Windows Server 2022 Datacenter: Azure-Edition – Gen 2**|
    |Username|**Kursteilnehmer**|
    |Kennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 02 > Übung 2 > Aufgabe 1 > Schritt 3 erstellt haben.**|
    |Öffentliche Eingangsports|**None**|
    |Sie verfügen bereits über eine Windows Server-Lizenz|**Nicht ausgewählt**|

    >**Hinweis**: Für öffentliche eingehende Ports verwenden wir die vorab erstellte Netzwerksicherheitsgruppe. 

4. Klicken Sie auf **Weiter: Datenträger >** , und legen Sie auf dem Blatt **Virtuellen Computer erstellen** auf der Registerkarte **Datenträger** den **Typ des Betriebssystemdatenträgers** auf **HDD Standard** fest, und klicken Sie dann auf **Weiter: Netzwerk >** .

5. Klicken Sie auf **Weiter: Netzwerk >** . Geben Sie auf dem Blatt **Virtuellen Computer erstellen** auf der Registerkarte **Netzwerk** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    |Einstellung|Wert|
    |---|---|
    |Virtuelles Netzwerk|**myVirtualNetwork**|
    |Subnet|**Privat (10.0.1.0/24)**|
    |Öffentliche IP-Adresse|**(new)myVmPrivate-ip**|
    |NIC-Netzwerksicherheitsgruppe|**None**|

6. Klicken Sie auf **Weiter: Verwaltung >** . Übernehmen Sie auf dem Blatt **Virtuellen Computer erstellen** auf der Registerkarte **Verwaltung** die Standardeinstellungen, und klicken Sie auf **Überprüfen und erstellen**.

7. Vergewissern Sie sich auf dem Blatt **Überprüfen und erstellen**, dass die Überprüfung erfolgreich war, und klicken Sie auf **Erstellen**.

    >**Hinweis**: Der zweite virtuelle Computer wird mit dem öffentlichen Subnetz verbunden.

8. Klicken Sie auf dem Blatt **Virtuelle Computer** auf **+ Hinzufügen**, und klicken Sie in der Dropdownliste auf **+ Virtueller Azure-Computer**.

9. Geben Sie auf dem Blatt **Virtuellen Computer erstellen** auf der Registerkarte **Grundeinstellungen** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    |Einstellung|Wert|
    |---|---|
    |Subscription|Der Name des Azure-Abonnements, das Sie in diesem Lab verwenden.|
    |Resource group|**AZ500LAB12**|
    |Name des virtuellen Computers|**myVmPublic**|
    |Region|**(USA) USA, Osten**|
    |Image|**Windows Server 2022 Datacenter: Azure-Edition – Gen 2**|
    |Username|**Kursteilnehmer**|
    |Kennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 02 > Übung 1 > Aufgabe 1 > Schritt 9 erstellt haben.**|
    |Öffentliche Eingangsports|**None**|
    |Sie verfügen bereits über eine Windows Server-Lizenz|**Nicht ausgewählt**|

    >**Hinweis**: Für öffentliche eingehende Ports verwenden wir die vorab erstellte Netzwerksicherheitsgruppe. 

10. Klicken Sie auf **Weiter: Datenträger >** , und legen Sie auf dem Blatt **Virtuellen Computer erstellen** auf der Registerkarte **Datenträger** den **Typ des Betriebssystemdatenträgers** auf **HDD Standard** fest, und klicken Sie dann auf **Weiter: Netzwerk >** .

11. Klicken Sie auf **Weiter: Netzwerk >** . Geben Sie auf dem Blatt **Virtuellen Computer erstellen** auf der Registerkarte **Netzwerk** die folgenden Einstellungen an (übernehmen Sie die Standardwerte für andere Einstellungen):

    |Einstellung|Wert|
    |---|---|
    |Virtuelles Netzwerk|**myVirtualNetwork**|
    |Subnet|**Öffentlich (10.0.0.0/24)**|
    |Öffentliche IP-Adresse|**(new)myVmPublic-ip**|
    |NIC-Netzwerksicherheitsgruppe|**None**|

12. Klicken Sie auf **Weiter: Verwaltung >** . Übernehmen Sie auf dem Blatt **Virtuellen Computer erstellen** auf der Registerkarte **Verwaltung** die Standardeinstellungen, und klicken Sie auf **Überprüfen und erstellen**.

13. Vergewissern Sie sich auf dem Blatt **Überprüfen und erstellen**, dass die Überprüfung erfolgreich war, und klicken Sie auf **Erstellen**.

    >**Hinweis**: Sobald die Bereitstellung des virtuellen Azure VM **myVMPublic** abgeschlossen ist, können Sie mit der nächsten Aufgabe fortfahren.

#### Aufgabe 7: Testen der Speicherverbindung vom privaten Subnetz, um sicherzustellen, dass der Zugriff zulässig ist

In dieser Aufgabe stellen Sie über Remotedesktop eine Verbindung mit dem virtuellen Computer „myVMPrivate“ her und ordnen der Dateifreigabe ein Laufwerk zu. 

1. Navigieren Sie zurück zum Blatt **Virtuelle Computer**. 

2. Klicken Sie auf dem Blatt **Virtuelle Computer** auf den Eintrag **myVMPrivate**.

3. Klicken Sie auf dem Blatt **myVMPrivate** auf **Verbinden** und dann im Dropdownmenü auf **Verbinden**. 

4. Laden Sie die RDP-Datei herunter, und stellen Sie damit über Remotedesktop eine Verbindung mit dem virtuellen Azure-Computer **myVMPrivate** her. Wenn Sie zur Authentifizierung aufgefordert werden, geben Sie die folgenden Anmeldeinformationen an:

    |Einstellung|Wert|
    |---|---|
    |Username|**Kursteilnehmer**|
    |Kennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 02 > Übung 2 > Aufgabe 1 > Schritt 3 erstellt haben.**|
     

    >**Hinweis**: Warten Sie, bis die Remotedesktopsitzung geöffnet und Server-Manager geladen wurde.

    >**Hinweis**: Jetzt ordnen Sie Laufwerk „Z“ einer Azure-Dateifreigabe in der Remotedesktop-Sitzung einem Windows Server 2022-Computer zu.

5. Klicken Sie in der Remotedesktopsitzung mit **myVMPrivate** auf **Start**, und klicken Sie dann auf **Windows PowerShell ISE**.

6. Öffnen Sie im Fenster **Windows PowerShell ISE** den Bereich **Skript**. Fügen Sie dann das zuvor in diesem Lab aufgezeichnete PowerShell-Skript ein, und führen Sie es aus. Das Skript weist das folgende Format auf:

    ```powershell
    $connectTestResult = Test-NetConnection -ComputerName <storage_account_name>.file.core.windows.net -Port 445
    if ($connectTestResult.TcpTestSucceeded) {
       # Save the password so the drive will persist on reboot
       cmd.exe /C "cmdkey /add:`"<storage_account_name>.file.core.windows.net`" /user:`"localhost\<storage_account_name>`"  /pass:`"<storage_account_key>`""
       # Mount the drive
       New-PSDrive -Name Z -PSProvider FileSystem -Root "\\<storage_account_name>.file.core.windows.net\my-file-share" -Persist
    } else {
       Write-Error -Message "Unable to reach the Azure storage account via port 445. Check to make sure your organization or ISP is not blocking port 445, or use Azure P2S VPN, Azure S2S VPN, or Express Route to tunnel SMB traffic over a different port."
    }
    ```
    >**Hinweis**: Der Platzhalter `<storage_account_name>` stellt den Namen des Speicherkontos dar, das die Dateifreigabe hostet, und `<storage_account_key>` einen zugehörigen Primärschlüssel.

7. Starten Sie den Datei-Explorer, und überprüfen Sie, ob die Zuordnung des Laufwerks „Z:“ erfolgreich erstellt wurde.

8. Führen Sie als Nächstes im Konsolenbereich der **Windows PowerShell ISE**-Konsole Folgendes aus, um sich zu vergewissern, dass der virtuelle Computer über keine ausgehende Verbindung mit dem Internet verfügt:

    ```powershell
    Test-NetConnection -ComputerName www.bing.com -Port 80
    ```

    >**Hinweis**: Der Test schlägt fehl, weil die dem privaten Subnetz zugeordnete Netzwerksicherheitsgruppe keinen ausgehenden Zugriff auf das Internet zulässt.

9. Beenden Sie die Remotedesktopsitzung mit dem virtuellen Azure-Computer **myVMPrivate**.

    >**Hinweis**: Zu diesem Zeitpunkt haben Sie sichergestellt, dass der virtuelle Computer im privaten Subnetz auf das Speicherkonto zugreifen kann. 

####  Aufgabe 8: Testen der Speicherverbindung vom öffentlichen Subnetz, um sicherzustellen, dass der Zugriff verweigert wird 

1. Navigieren Sie zurück zum Blatt **Virtuelle Computer**. 

2. Klicken Sie auf dem Blatt **Virtuelle Computer** auf den Eintrag **myVMPublic**.

3. Klicken Sie auf dem Blatt **myVMPublic** auf **Verbinden** und dann im Dropdownmenü auf **Verbinden**. 

4. Klicken Sie auf **Verbindung herstellen über RDP**, und stellen Sie damit über Remotedesktop eine Verbindung mit dem virtuellen Azure-Computer **myVMPublic** her. Wenn Sie zur Authentifizierung aufgefordert werden, geben Sie die folgenden Anmeldeinformationen an:

    |Einstellung|Wert|
    |---|---|
    |Username|**Kursteilnehmer**|
    |Kennwort|**Verwenden Sie Ihr persönliches Kennwort, das Sie in Lab 02 > Übung 2 > Aufgabe 1 > Schritt 3 erstellt haben.**|


    >**Hinweis**: Warten Sie, bis die Remotedesktopsitzung geöffnet und Server-Manager geladen wurde.

    >**Hinweis**: Jetzt ordnen Sie Laufwerk „Z“ einer Azure-Dateifreigabe in der Remotedesktop-Sitzung einem Windows Server 2022-Computer zu.

5. Klicken Sie in der Remotedesktopsitzung mit **myVMPublic** auf **Start**, und klicken Sie dann auf **Windows PowerShell ISE**.

6. Öffnen Sie im Fenster **Windows PowerShell ISE** den Bereich **Skript**. Fügen Sie dann dasselbe PowerShell-Skript ein, das Sie in der Remotedesktopsitzung mit dem virtuellen Azure-Computer **myVMPrivate** ausgeführt haben, und führen Sie das Skript aus.

    >**Hinweis**: Dieses Mal erhalten Sie den Fehler **New-PSDrive: Zugriff verweigert**. 

    >**Hinweis**: Der Zugriff wird verweigert, weil der virtuelle Computer *myVmPublic* im öffentlichen Subnetz bereitgestellt wird. Für das öffentliche Subnetz ist kein Dienstendpunkt für Azure Storage aktiviert. Das Speicherkonto lässt nur den Netzwerkzugriff über das private Subnetz zu.

7. Führen Sie als Nächstes im Konsolenbereich der **Windows PowerShell ISE**-Konsole Folgendes aus, um sich zu vergewissern, dass der virtuelle Computer über eine ausgehende Verbindung mit dem Internet verfügt: 

    ```powershell
    Test-NetConnection -ComputerName www.bing.com -Port 80
    ```

    >**Hinweis**: Der Test ist erfolgreich, da es keine Sicherheitsregel für ausgehenden Datenverkehr gibt, um das Internet im öffentlichen Subnetz zu verweigern.

8. Beenden Sie die Remotedesktopsitzung mit dem virtuellen Azure-Computer **myVMPublic**.

    >**Hinweis**: Zu diesem Zeitpunkt haben Sie sichergestellt, dass der virtuelle Computer im öffentlichen Subnetz nicht auf das Speicherkonto zugreifen kann, aber Zugriff auf das Internet hat.


**Bereinigen von Ressourcen**

> Denken Sie daran, alle neu erstellten Azure-Ressourcen zu entfernen, die Sie nicht mehr verwenden. Durch das Entfernen nicht verwendeter Ressourcen wird sichergestellt, dass keine unerwarteten Kosten anfallen. 

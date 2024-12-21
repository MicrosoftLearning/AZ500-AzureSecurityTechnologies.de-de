---
lab:
  title: 09 – Microsoft Defender for Cloud
  module: Module 03 - Manage security posture by using Microsoft Defender for Cloud
---

# Lab 09: Microsoft Defender für Cloud
# Lab-Handbuch für Kursteilnehmer

## Labszenario

Sie wurden gebeten, einen Proof of Concept der Microsoft Defender für Cloud-basierten Umgebung zu erstellen. Insbesondere geht es Ihnen um Folgendes:

- Konfigurieren von Microsoft Defender for Cloud mit erweiterten Sicherheitsfunktionen für Server zur Überwachung eines virtuellen Computers.
- Sehen Sie sich die Empfehlungen von Microsoft Defender für Cloud für den virtuellen Computer an.
- Implementieren Sie die Empfehlungen für die Gastkonfiguration und den Just-In-Time-VM-Zugriff. 
- Überprüfen, wie die Sicherheitsbewertung verwendet werden kann, um den Fortschritt bei der Erstellung einer sichereren Infrastruktur zu bestimmen.

> Für alle Ressourcen in diesem Lab verwenden wir die Region **USA, Osten**. Vergewissern Sie sich bei Ihrem Kursleiter, dass dies die Region ist, die für den Kurs verwendet werden soll. 

## Ziele des Labs

In diesem Lab führen Sie die folgende Übung aus:

- Übung 1: Implementieren von Microsoft Defender für Cloud

## Microsoft Defender für Cloud-Diagramm

![image](https://github.com/MicrosoftLearning/AZ500-AzureSecurityTechnologies/assets/91347931/c31055cc-de95-41f6-adef-f09d756a68eb)

## Anweisungen

### Übung 1: Implementieren von Microsoft Defender für Cloud

In dieser Übung führen Sie die folgenden Aufgaben aus:

- Aufgabe 1: Konfigurieren von Microsoft Defender für Cloud
- Aufgabe 2: Überprüfen der Empfehlungen von Microsoft Defender für Cloud
- Aufgabe 3: Implementieren der Empfehlung von Microsoft Defender for Cloud zum Aktivieren des Just-in-Time-VM-Zugriffs

#### Übung 1: Konfigurieren von Microsoft Defender for Cloud Enhanced Security Features for Servers

Bei dieser Aufgabe werden Sie Microsoft Defender für Cloud Enhanced Security Features for Servers einbinden und konfigurieren.

1. Starten Sie eine Browsersitzung und melden Sie sich bei Ihrem [Azure-Abonnement](https://azure.microsoft.com/en-us/free/?azure-portal=true) an. in dem Sie über Administratorzugriff verfügen.

2. Geben Sie im Azure-Portal in das Textfeld „Ressourcen, Dienste und Dokumente suchen“ oben auf der Azure-Portalseite Microsoft Defender for Cloud ein und drücken Sie dann die Eingabetaste.

3. Gehen Sie auf Microsoft Defender for Cloud, Verwaltungsblatt, zu den Umgebungseinstellungen. Erweitern Sie die Ordner für die Umgebungseinstellungen, bis der Abschnitt Abonnement angezeigt wird, und klicken Sie dann auf das Abonnement, um Details anzuzeigen.

4. Erweitern Sie im Blatt Einstellungen unter Defender-Pläne Cloud Workload Protection (CWP)“.
  
5. Wählen Sie in der Liste Cloud Workload Protection (CWP)-Plan die Option Server aus. Ändern Sie auf der rechten Seite den Status von Aus in Ein, und klicken Sie dann auf Speichern.
  
6. Um die Details von Microsoft Defender for Servers Plan 2 zu überprüfen, wählen Sie Plan ändern >.

>**Hinweis**: Wenn Sie den Cloud Workload Protection (CWP)-Serverplan von „Aus“ auf „Ein“ setzen, wird der „Microsoft Defender for Servers Plan 2“ aktiviert.

#### Aufgabe 2: Überprüfen der Empfehlung von Microsoft Defender für Cloud

In dieser Aufgabe überprüfen Sie die Empfehlungen von Microsoft Defender für Cloud. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Microsoft Defender für Cloud \| Übersicht**. 

2. Überprüfen Sie auf dem Blatt **Microsoft Defender für Cloud \| Übersicht** die Kachel **Sicherheitsbewertung**.

    >**Hinweis**: Notieren Sie sich die aktuelle Bewertung, sofern verfügbar.

3. Navigieren Sie zurück zum Blatt **Microsoft Defender for Cloud \| Übersicht** und wählen Sie **Bewertete Ressourcen** aus.

4. Wählen Sie auf dem Blatt **Bestand** den Eintrag **myVM** aus.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten und die Browserseite aktualisieren, bis der Eintrag angezeigt wird.
    
5. Überprüfen Sie auf dem Blatt **Ressourcenzustand** auf der Registerkarte **Empfehlungen** die Liste der Empfehlungen für **myVM**.

#### Aufgabe 3: Implementieren der Empfehlung von Microsoft Defender for Cloud zum Aktivieren des Just-in-Time-VM-Zugriffs

In dieser Aufgabe implementieren Sie die Empfehlung von Microsoft Defender for Cloud, um den Just-in-Time-VM-Zugriff zu aktivieren. 

1. Navigieren Sie im Azure-Portal zurück zum Blatt **Microsoft Defender for Cloud \| Übersicht** und wählen Sie dort die Option **Workloadschutz** aus (zu finden unter **Cloudsicherheit**).

2. Scrollen Sie auf dem Blatt **Microsoft Defender for Cloud \| Workloadschutz** nach unten zum Abschnitt **Erweiterter Schutz** und klicken Sie auf die Kachel **Just-in-Time-VM-Zugriff**.

3. Wählen Sie auf dem Blatt **Just-in-Time-VM-Zugriff** im Abschnitt **VMs** die Option **Nicht konfiguriert** aus und klicken Sie dann auf den Eintrag **myVM**.

    >**Hinweis**: Möglicherweise müssen Sie einige Minuten warten, die Browserseite aktualisieren und **Nicht konfiguriert** auswählen, dass der Eintrag angezeigt wird.

4. Klicken Sie ganz rechts im Abschnitt **Virtuelle Computer** auf die Option **JIT auf 1 VM** aktivieren.

5. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** ganz rechts in der Zeile, die auf Port **22** verweist, auf die Schaltfläche mit den Auslassungspunkten und dann auf **Löschen**.

6. Klicken Sie auf dem Blatt **Konfiguration des JIT-VM-Zugriffs** auf **Speichern**.

    >**Hinweis**: Überwachen Sie den Status der Konfiguration, indem Sie auf der Symbolleiste auf das Symbol **Benachrichtigungen** klicken, um das Blatt **Benachrichtigungen** anzuzeigen. 

    >**Hinweis**: Es kann einige Zeit dauern, bis die Implementierung von Empfehlungen in diesem Lab durch die Sicherheitsbewertung widergespiegelt wird. Überprüfen Sie regelmäßig die Sicherheitsbewertung, um die Auswirkungen der Implementierung dieser Features zu ermitteln. 

> Ergebnisse: Sie haben Microsoft Defender für Cloud eingeführt und Empfehlungen für virtuelle Computer implementiert. 

    >**Note**: Do not remove the resources from this lab as they are needed for the Microsoft Sentinel lab.

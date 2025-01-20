---
lab:
  title: '10: Just-in-Time-Zugriff auf VMs ermöglichen'
  module: Module 03 - Configure and manage threat protection by using Microsoft Defender for Cloud
---

# Lab 10: Just-in-Time-Zugriff auf VMs ermöglichen

# Lab-Handbuch für Kursteilnehmende

## Labszenario

Als technische Fachkraft für Sicherheit in Azure bei einem Finanzdienstleistungsunternehmen sind Sie für die Sicherung von Azure-Ressourcen verantwortlich, einschließlich Virtual Machines (VMs), die kritische Anwendungen hosten. Das Sicherheitsteam hat festgestellt, dass ein ständiger offener Zugang zu VMs das Risiko von Brute-Force-Angriffen und unbefugtem Zugriff erhöht. Um dies zu mildern, hat der Chief Information Security Officer (CISO) Sie gebeten, den Just-in-Time (JIT)-VM-Zugriff auf einer bestimmten Azure-VM zu aktivieren, die für die Verarbeitung von Finanztransaktionen verwendet wird.

## Ziele des Labs

In diesem Lab führen Sie die folgenden Übungen aus:

- Übung 1: Aktivieren Sie JIT auf Ihren VMs über das Azure-Portal.

- Übung 2: Fordern Sie über das Azure-Portal Zugriff auf eine VM an, für die JIT aktiviert ist.

## Übungsanweisungen 

### Übung 1: Aktivieren Sie JIT auf Ihren VMs von Azure Virtual Machines aus

>**Hinweis**: Sie können JIT auf einer VM über die Seiten für virtuelle Azure-Computer des Azure-Portals aktivieren.

1. Geben Sie im Suchfeld oben im Portal den Begriff **Virtuelle Computer** ein. Wählen Sie in den Suchergebnissen **Virtuelle Computer** aus.

2. Wählen Sie **myVM** aus.
 
3. Wählen Sie **Konfiguration** aus dem Abschnitt **Einstellungen** von **myVM** aus.
   
4. Wählen Sie unter **Just-In-Time-VM-Zugriff** die Option **Just-In-Time aktivieren** aus.

5. Klicken Sie unter **Just-In-Time-Zugriff auf VMs** auf den Link **Microsoft Defender for Cloud öffnen**.

6. Standardmäßig gelten für den Just-in-Time-Zugriff auf die VM die folgenden Einstellungen:

   - Windows-Computer
   
     - RDP-Port: 3389
     - Maximal erlaubte Zugriffsdauer: drei Stunden
     - Zulässige Quell-IP-Adressen: beliebige

   - Linux-Computer
     - SSH-Port: 22
     - Maximal erlaubte Zugriffsdauer: drei Stunden
     - Zulässige Quell-IP-Adressen: beliebige
   
7. Standardmäßig gelten für den Just-in-Time-Zugriff auf die VM die folgenden Einstellungen:

   - Klicken Sie auf der Registerkarte **Konfiguriert** mit der rechten Maustaste auf die VM, zu der Sie einen Port hinzufügen möchten, und wählen Sie „Bearbeiten“ aus.

   ![image](https://github.com/user-attachments/assets/aa4ded55-c5b1-4d40-b5a0-a4c33b9eb81b)
   
   - Unter **JIT-VM-Zugriffskonfiguration** können Sie die vorhandenen Einstellungen eines bereits geschützten Ports bearbeiten oder einen neuen benutzerdefinierten Port hinzufügen.
   - Wenn Sie die Bearbeitung der Ports abgeschlossen haben, wählen Sie **Speichern** aus.   

### Übung 2: Fordern Sie über die Verbindungsseite von Azure Virtual Machines Zugriff auf eine JIT-fähige VM an.

>**Hinweis**: Wenn JIT für eine VM aktiviert ist, müssen Sie zum Herstellen der Verbindung entsprechend Zugriff anfordern. Sie können Zugriff auf jede der unterstützten Arten anfordern, unabhängig davon, wie Sie JIT aktiviert haben.
   
1. Öffnen Sie im Azure-Portal die Seiten mit den virtuellen Computern.

2. Wählen Sie die VM aus, mit der Sie eine Verbindung herstellen möchten, und öffnen Sie die Seite **Verbinden**.

   - Azure prüft, ob JIT auf diesem virtuellen Computer aktiviert ist.

        - Wenn JIT für den virtuellen Computer nicht aktiviert ist, werden Sie aufgefordert, es zu aktivieren.
    
        - Wenn JIT aktiviert ist, wählen Sie **Zugriff anfordern** aus, um eine Zugriffsanforderung mit der anfordernden IP-Adresse, dem Zeitbereich und den Ports zu übergeben, die für diese VM konfiguriert wurden.
    
   ![image](https://github.com/user-attachments/assets/f5d0b67c-7731-4261-b0eb-a56c505dadd4)

> **Ergebnisse**: Sie haben verschiedene Methoden zum Aktivieren von JIT auf Ihren VMs und zum Anfordern von Zugriff auf VMs untersucht, bei denen JIT in Microsoft Defender for Cloud aktiviert ist.

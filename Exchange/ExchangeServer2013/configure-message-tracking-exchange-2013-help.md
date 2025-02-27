---
title: 'Configure message tracking: Exchange 2013 Help'
TOCTitle: Configure message tracking
ms:assetid: 50eb5213-cf27-4179-b427-38d751ee4a70
ms:mtpsurl: https://technet.microsoft.com/library/Aa997984(v=EXCHG.150)
ms:contentKeyID: 50646521
ms.reviewer: 
manager: serdars
ms.author: serdars
ms.topic: article
description: How to configure message tracking in Exchange Server
author: msdmaguire
f1.keywords:
- NOCSH
mtps_version: v=EXCHG.150
---

# Configure message tracking

_**Applies to:** Exchange Server 2013_

Message tracking records the SMTP transport activity of all messages transferred to and from the Transport service or mailboxes on a Microsoft Exchange Server 2013 Mailbox server. You can use message tracking logs for message forensics, mail flow analysis, reporting, and troubleshooting.

## What do you need to know before you begin?

- Estimated time to complete: 15 minutes

- You need to be assigned permissions before you can perform this procedure or procedures. To see what permissions you need, see the "Transport service" entries in the [Mail flow permissions](mail-flow-permissions-exchange-2013-help.md) topic or the "Mailbox server configuration" entry in the [Recipients Permissions](recipients-permissions-exchange-2013-help.md) topic.

- You can use the Exchange admin center (EAC) to enable or disable message tracking, or set the message tracking log path. For all other message tracking options, you need to use the Exchange Management Shell.

- On an Exchange 2013 Mailbox server, you can use either the **Set-TransportService** or the **Set-MailboxServer** cmdlet to configure the message tracking options. The procedures in this topic use the **Set-TransportService** cmdlet.

- For information about keyboard shortcuts that may apply to the procedures in this topic, see [Keyboard shortcuts in the Exchange admin center](keyboard-shortcuts-in-the-exchange-admin-center-2013-help.md).

> [!TIP]
> Having problems? Ask for help in the Exchange forums. Visit the forums at [Exchange Server](https://social.technet.microsoft.com/forums/office/home?category=exchangeserver).

## Use the EAC to configure message tracking on Mailbox servers

1. In the EAC, navigate to **Servers** \> **Servers**.

2. Select the Mailbox server you want to configure, and then click **Edit** ![Edit icon.](images/JJ218640.6f53ccb2-1f13-4c02-bea0-30690e6ea71d(EXCHG.150).gif "Edit icon").

3. On the server properties page, click **Transport Logs**.

4. In the **Message tracking log** section, change any of the following:

   - **Enable message tracking log**: To disable message tracking on the server, clear the check box. To enable message tracking on the server, select the check box.

   - **Message tracking log path**: The value you specify must be on the local Exchange server. If the folder doesn't exist, it will be created for you when you click **Save**.

5. Click **Save**.

## Use the Shell to configure message tracking

To configure message tracking, run the following command:

```powershell
Set-TransportService <ServerIdentity> -MessageTrackingLogEnabled <$true | $false> -MessageTrackingLogMaxAge <dd.hh:mm:ss> -MessageTrackingLogMaxDirectorySize <Size> -MessageTrackingLogMaxFileSize <Size> -MessageTrackingLogPath <LocalFilePath> -MessageTrackingLogSubjectLoggingEnabled <$true|$false>
```

This example sets the following message tracking log settings on the Mailbox server named Mailbox01:

- Sets the location of the message tracking log files to D:\\Message Tracking Log. Note that if the folder doesn't exist, it will be created for you.

- Sets the maximum size of a message tracking log file to 20 MB.

- Sets the maximum size of the message tracking log directory to 1.5 GB.

- Sets the maximum age of a message tracking log file to 45 days.

```powershell
Set-TransportService Mailbox01 -MessageTrackingLogPath "D:\Hub Message Tracking Log" -MessageTrackingLogMaxFileSize 20MB -MessageTrackingLogMaxDirectorySize 1.5GB -MessageTrackingLogMaxAge 45.00:00:00
```

> [!NOTE]
> <UL>
> <LI>
> <P>Setting the <EM>MessageTrackingLogPath</EM> parameter to the value <CODE>$null</CODE>, effectively disables message tracking. However, if the value of the <EM>MessageTrackingLogEnabled</EM> parameter is <CODE>$true</CODE>, event log errors are generated.</P>
> <LI>
> <P>Setting the <EM>MessageTrackingLogMaxAge</EM> parameter to the value <CODE>00:00:00</CODE> prevents the automatic removal of message tracking log files because of their age.</P>
> <LI>
> <P>On Exchange 2013 Mailbox servers, the maximum size of the message tracking log directory is three times the value of the <EM>MessageTrackingLogMaxDirectorySize</EM> parameter. Although the message tracking log files that are generated by the four different services have four different name prefixes, the amount and frequency of data written to the <STRONG>MSGTRKMA</STRONG> log files is negligible compared to the three other log file prefixes. For more information, see the "Structure of the message tracking log files" section in the <A href="message-tracking-exchange-2013-help.md">Message tracking</A> topic.</P></LI></UL>

This example disables message subject logging in the message tracking log on the Mailbox server named Mailbox01:

```powershell
Set-TransportService Mailbox01 -MessageTrackingLogSubjectLoggingEnabled $false
```

This example disables message tracking on the Mailbox server named Mailbox01:

```powershell
Set-TransportService Mailbox01 -MessageTrackingLogEnabled $false
```

## How do you know this worked?

To verify that you have successfully configured message tracking, do the following:

1. In the Shell, run the following command:

    ```powershell
    Get-TransportService <ServerIdentity> | Format-List MessageTrackingLog*
    ```

2. Verify that the values displayed are the values you configured.

---
title: OID_WDI_ABORT_TASK (dot11wificxintf.h)
description: The OID_WDI_ABORT_TASK command is a property that is sent down to cancel a specific pending task.
ms.date: 07/31/2021
keywords:
 - OID_WDI_ABORT_TASK Network Drivers Starting with Windows Vista
---

# OID\_WDI\_ABORT\_TASK (dot11wificxintf.h)

[!INCLUDE [WiFiCx topic note](../includes/wificx-version-warning.md)]


OID\_WDI\_ABORT\_TASK is a property that is sent down to cancel a specific pending task.

| Scope | Set serialized with task | Normal execution time (seconds) |
|-------|--------------------------|---------------------------------|
| Port  | No                       | 1                               |

 

This command follows property semantics. It should be treated as a signal, should be handled as quickly as possible, and should be completed independently of task completion. The IHV component must then attempt to complete the pending task as soon as possible.

## Command parameters


| TLV                                                                    | Multiple TLV instances allowed | Optional | Description                                          |
|------------------------------------------------------------------------|--------------------------------|----------|------------------------------------------------------|
| [**WDI\_TLV\_CANCEL\_PARAMETERS**](./wdi-tlv-cancel-parameters.md) |                                |          | Information for the command that is being cancelled. |

 

## Command result


Contains a status of NDIS\_STATUS\_SUCCESS. There is no additional payload.
## Examples


Original input task command:

|Field|Subfield|Type|Value|
|--- |--- |--- |--- |
|NDIS\_OID\_REQUEST|Oid|NDIS\_OID|OID(WDI\_TASK\_SCAN)|
|--- |InputBufferLength|UINT32|0x210 (example)|
|---|InformationBuffer|PVOID|Pointer to memory block containing WDI\_MESSAGE\_HEADER + TLV payload|
|WDI\_MESSAGE\_HEADER|PortId|UINT16|0x0001 (example)|
|--- |Reserved|UINT16|N/A|
|--- |WiFiStatus|NDIS\_STATUS|N/A|
|--- |TransactionId|UINT32|0x1111 (example)|
|--- |IhvSpecificId|UINT32|N/A|
|TLV Payload|TLV Payload|Various|Payload data|
 

Abort task input command (with message header):

|Field|Subfield|Type|Value|
|--- |--- |--- |--- |
|NDIS\_OID\_REQUEST|Oid|NDIS\_OID|OID(WDI\_ABORT\_TASK)|
|--- |InputBufferLength|UINT32|sizeof(WDI\_MESSAGE\_HEADER) + sizeof(WDI\_TLV\_CANCEL\_PARAMETERS)|
|---|InformationBuffer|PVOID|Pointer to memory block containing WDI\_MESSAGE\_HEADER + TLV payload|
|WDI\_MESSAGE\_HEADER|PortId|UINT16|0x0001 (example)|
|--- |Reserved|UINT16|N/A|
|--- |WiFiStatus|NDIS\_STATUS|N/A|
|--- |TransactionId|UINT32|0x2222 (example)|
|--- |IhvSpecificId|UINT32|0|
|WDI\_TLV\_CANCEL\_PARAMETERS|OriginalTaskOid|NDIS\_OID|OID(WDI\_TASK\_SCAN)|
|--- |OriginalPortId|UINT16|0x0001 (example)|
|--- |OriginalTransactionId|UINT32|0x1111 (example)|
 

Abort task command result:

|Field|Subfield|Type|Value|
|--- |--- |--- |--- |
|NDIS\_OID\_REQUEST|Oid|NDIS\_OID|OID(WDI\_TASK\_SCAN)|
|---|OutputBufferLength|UINT32|sizeof(WDI\_MESSAGE\_HEADER)|
|---|InformationBuffer|PVOID|Pointer to memory block containing WDI\_MESSAGE\_HEADER|
|WDI\_MESSAGE\_HEADER|PortId|UINT16|0x0001 (example)|
|---|Reserved|UINT16|N/A|
|---|WiFiStatus|NDIS\_STATUS|NDIS\_STATUS\_SUCCESS|
|---|TransactionId|UINT32|0x2222 (example)|
|---|IhvSpecificId|UINT32|N/A|
 

## Requirements

|Requirement|Value|
|--- |--- |
|Minimum supported client|Windows 11|
|Minimum supported server|Windows Server 2022|
|Header|dot11wificxintf.h|

 


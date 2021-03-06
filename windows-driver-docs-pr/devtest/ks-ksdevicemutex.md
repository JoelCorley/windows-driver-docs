---
title: KsDeviceMutex rule ()
description: The KsDeviceMutex rule specifies that a kernel streaming miniport driver uses KsAcquireDevice and KsReleaseDevice in the correct sequence. That is, every call to KsAcquireDevice must have a corresponding call to KsReleaseDevice.
ms.assetid: 6F69B273-6780-4A01-8266-2B056E4F2C84
ms.author: windowsdriverdev
ms.date: 5/21/2018
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
keywords: ["KsDeviceMutex rule ()"]
topic_type:
- apiref
api_name:
- KsDeviceMutex
api_type:
- NA
---

# KsDeviceMutex rule ()


The **KsDeviceMutex** rule specifies that a kernel streaming miniport driver uses [**KsAcquireDevice**](https://msdn.microsoft.com/library/windows/hardware/ff560911) and [**KsReleaseDevice**](https://msdn.microsoft.com/library/windows/hardware/ff566783) in the correct sequence. That is, every call to **KsAcquireDevice** must have a corresponding call to **KsReleaseDevice**.

|              |     |
|--------------|-----|
| Driver model | KS  |

|                                   |                                                                                                                                       |
|-----------------------------------|---------------------------------------------------------------------------------------------------------------------------------------|
| Bug check(s) found with this rule | [**Bug Check 0xC4: DRIVER\_VERIFIER\_DETECTED\_VIOLATION**](https://msdn.microsoft.com/library/windows/hardware/ff560187) (0x00081001) |

How to test
-----------

<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">At run time</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>To verify this rule, open a Command Prompt window. Enter a Driver Verifier command and specify <strong>/domain ks</strong>.</p>
<p>For example:</p>
<p><strong>verifier /domain ks</strong> [<em>options</em>] <strong>/driver</strong> <em>&lt;yourdriver&gt;</em></p>
<p>For more information, see [Driver Verifier](https://msdn.microsoft.com/library/windows/hardware/ff545448).</p></td>
</tr>
</tbody>
</table>

 

 

 






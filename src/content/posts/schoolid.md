+++
title = "Reverse Engineering The School's ID Card"
date = "2025-10-31T22:06:00+07:00"
tags = ["RFID"]
description = "Dumping and also cloning it :p"
draft = true
+++

So I was wondering if the school id can somehow be "hacked" and also RFID in general, and that made me fall into the rabbit hole of RFID stuff.
I guess that 1 more thing to ~~waste~~ spend money on instead of maimai :3

Most of the stuff I found is from the Chameleon Ultra ("CU from now") but I'll format the commands to be pm3 commands since it seems more used.

So what is the chip inside this thing anyways?

```
[usb] pm3 --> auto
[=] lf search

[=] Note: False Positives ARE possible
[=]
[=] Checking for known tags...
[=]
[-] ⛔ No known 125/134 kHz tags found!
[=] Couldn't identify a chipset
[=] hf search
 🕖  Searching for ISO14443-A tag...
[=] ---------- ISO14443-A Information ----------
[+]  UID: XX XX XX XX   ( ONUID, re-used )
[+] ATQA: 00 04
[+]  SAK: 08 [2]
[+] Possible types:
[+]    MIFARE Classic 1K
[=]
[=] Proprietary non iso14443-4 card found
[=] RATS not supported
[#] Static nonce....... 01200145
[+] Static nonce....... yes

[?] Hint: Try `hf mf info`


[+] Valid ISO 14443-A tag found
```

```
[usb] pm3 --> hf mf info

[=] --- ISO14443-a Information -----------------------------
[+]  UID: XX XX XX XX
[+] ATQA: 00 04
[+]  SAK: 08 [1]

[=] --- Keys Information
[+] loaded 2 user keys
[+] loaded 61 hardcoded keys
[+] Sector 0 key A... FFFFFFFFFFFF
[+] Sector 0 key B... FFFFFFFFFFFF
[+] Sector 1 key A... FFFFFFFFFFFF
[+] Sector 1 key B... FFFFFFFFFFFF
[+] Block 0.......... XXXXXXXXXX0804006263646566676869 | bcdefghi

[=] --- Fingerprint
[+] Fudan based card

[=] --- Magic Tag Information
[=] <n/a>

[=] --- PRNG Information
[#] Static nonce....... 01200145
[+] Static nonce... yes
```

So it's a Fudan's MIFARE Classic 1K. I kinda expect a Fudan here since NXP ICs isn't common. And since this is a IC that's cracked since 2008, it's not that hard to copy this thing. We still don't know the keys tho. So I ran `hf mf autopwn`

```
[usb] pm3 --> hf mf autopwn

[!] ⚠️  Known key failed. Can't authenticate to block   0 key type A
[#] Static nonce....... 01200145
[!] ⚠️  No known key was supplied, key recovery might fail
[+] loaded 5 user keys
[+] loaded 61 hardcoded keys
[=] Running strategy 1
[=] Running strategy 2
[+] Target sector   0 key type A -- found valid key [ FFFFFFFFFFFF ] (used for nested / hardnested attack)
[+] Target sector   0 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   1 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   1 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   2 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   2 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   3 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   3 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   4 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   4 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   5 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   5 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   7 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   7 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   8 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   8 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   9 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector   9 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  10 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  10 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  11 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  11 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  12 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  12 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  13 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  13 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  14 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  14 key type B -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  15 key type A -- found valid key [ FFFFFFFFFFFF ]
[+] Target sector  15 key type B -- found valid key [ FFFFFFFFFFFF ]

[+] Found 1 key candidates
[+] target block   24 key type A -- found valid key [ 9DCEF75F7653 ]
[+] Target sector   6 key type A -- found valid key [ 9DCEF75F7653 ]

[+] Found 1 key candidates
[+] target block   24 key type B -- found valid key [ 9DCEF75F7653 ]
[+] Target sector   6 key type B -- found valid key [ 9DCEF75F7653 ]

[+] -----+-----+--------------+---+--------------+----
[+]  Sec | Blk | key A        |res| key B        |res
[+] -----+-----+--------------+---+--------------+----
[+]  000 | 003 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  001 | 007 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  002 | 011 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  003 | 015 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  004 | 019 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  005 | 023 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  006 | 027 | 9DCEF75F7653 | C | 9DCEF75F7653 | C
[+]  007 | 031 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  008 | 035 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  009 | 039 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  010 | 043 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  011 | 047 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  012 | 051 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  013 | 055 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  014 | 059 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+]  015 | 063 | FFFFFFFFFFFF | D | FFFFFFFFFFFF | D
[+] -----+-----+--------------+---+--------------+----
[=] ( D:Dictionary / S:darkSide / U:User / R:Reused / N:Nested / H:Hardnested / C:statiCnested / A:keyA  )
```

Took less than 5 seconds to crack the keys. The only sector that have the key changed here is sector 6. What is in there?

```
{
  "Created": "proxmark3",
  "FileType": "mfc v2",
  "Card": {
    "UID": "XXXXXXXX",
    "ATQA": "0400",
    "SAK": "08"
  },
  "blocks": {
    "0": "XXXXXXXXXX0804006263646566676869",
    "1": "00000000000000000000000000000000",
    "2": "00000000000000000000000000000000",
    "3": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "4": "00000000000000000000000000000000",
    "5": "00000000000000000000000000000000",
    "6": "00000000000000000000000000000000",
    "7": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "8": "00000000000000000000000000000000",
    "9": "00000000000000000000000000000000",
    "10": "00000000000000000000000000000000",
    "11": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "12": "00000000000000000000000000000000",
    "13": "00000000000000000000000000000000",
    "14": "00000000000000000000000000000000",
    "15": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "16": "00000000000000000000000000000000",
    "17": "00000000000000000000000000000000",
    "18": "00000000000000000000000000000000",
    "19": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "20": "00000000000000000000000000000000",
    "21": "00000000000000000000000000000000",
    "22": "00000000000000000000000000000000",
    "23": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "24": "00000000000000000000000000000000",
    "25": "00000000000000000000000000000000",
    "26": "YYYYYYYYYYYYYYYYYYYYYY0000000000",
    "27": "9DCEF75F7653FF0780699DCEF75F7653",
    "28": "00000000000000000000000000000000",
    "29": "00000000000000000000000000000000",
    "30": "00000000000000000000000000000000",
    "31": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "32": "00000000000000000000000000000000",
    "33": "00000000000000000000000000000000",
    "34": "00000000000000000000000000000000",
    "35": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "36": "00000000000000000000000000000000",
    "37": "00000000000000000000000000000000",
    "38": "00000000000000000000000000000000",
    "39": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "40": "00000000000000000000000000000000",
    "41": "00000000000000000000000000000000",
    "42": "00000000000000000000000000000000",
    "43": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "44": "00000000000000000000000000000000",
    "45": "00000000000000000000000000000000",
    "46": "00000000000000000000000000000000",
    "47": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "48": "00000000000000000000000000000000",
    "49": "00000000000000000000000000000000",
    "50": "00000000000000000000000000000000",
    "51": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "52": "00000000000000000000000000000000",
    "53": "00000000000000000000000000000000",
    "54": "00000000000000000000000000000000",
    "55": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "56": "00000000000000000000000000000000",
    "57": "00000000000000000000000000000000",
    "58": "00000000000000000000000000000000",
    "59": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF",
    "60": "00000000000000000000000000000000",
    "61": "00000000000000000000000000000000",
    "62": "00000000000000000000000000000000",
    "63": "FFFFFFFFFFFFFF078069FFFFFFFFFFFF"
  },
  "SectorKeys": {
    "0": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block0": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block1": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block2": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block3": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "1": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block4": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block5": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block6": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block7": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "2": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block8": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block9": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block10": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block11": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "3": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block12": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block13": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block14": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block15": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "4": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block16": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block17": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block18": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block19": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "5": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block20": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block21": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block22": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block23": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "6": {
      "KeyA": "9DCEF75F7653",
      "KeyB": "9DCEF75F7653",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block24": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block25": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block26": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block27": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "7": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block28": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block29": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block30": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block31": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "8": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block32": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block33": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block34": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block35": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "9": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block36": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block37": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block38": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block39": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "10": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block40": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block41": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block42": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block43": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "11": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block44": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block45": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block46": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block47": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "12": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block48": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block49": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block50": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block51": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "13": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block52": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block53": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block54": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block55": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "14": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block56": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block57": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block58": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block59": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    },
    "15": {
      "KeyA": "FFFFFFFFFFFF",
      "KeyB": "FFFFFFFFFFFF",
      "AccessConditions": "FF078069",
      "AccessConditionsText": {
        "block60": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block61": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block62": "read AB; write AB; increment AB; decrement transfer restore AB",
        "block63": "write A by A; read/write ACCESS by A; read/write B by A",
        "UserData": "69"
      }
    }
  }
}
```

So on block 26 (Y) there `AAAAA|BBBBB` in hex. A being the student ID for that card and B is a 5 digit "passcode". The passcode is to probably identify a suspended card.

Pretty much this thing is cracked already and ready to be copied. So I tried copying it to other tags and testing if it work. And this is what I tested as of writing this.

- Gen 1a Magic Card = Not enough testing but seems to not work sometimes.
- CUID/Gen 2 Magic Card = Works perfectly.
- CU Emulation = Works with some error but mostly perfect.

So... If ya don't want to pay 100 THB for a new card, just back up your card :p
You dont even need the hardware. Just a Android phone with "Mifare Classic Tool" and a magic tag is enough. I've already mentioned the keys here. ( Not on iOS tho :) )
But like if your card is indeed lost and you disabled the card, your copied card would be disabled too :(

Anyways we just have to see if ACT with change the card to something a Desfire or something. (Also the 100 THB would also go up if they changed it since MFC 1K is around 10 THB per card while a Desfire is like 50 THB ;-; )

Anyways I'm out
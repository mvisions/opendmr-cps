# CSV import / export

[中文](csv.md)

The format is the **OpenGD77 CPS**'s "File → CSV" format: files exported by the OpenGD77 CPS import as they are, and a
channel list from another CPS, edited into this format (in Excel or a text editor), imports too. What OpenDMR has more
is in the last columns of Channels.csv and in more Channel Type values.

## Export

"Export CSV" downloads a zip with:

| File | Content |
| --- | --- |
| Channels.csv | the channels, a row each |
| Zones.csv | zones: Zone Name, then Channel1 … Channel80 the channel names |
| Contacts.csv | contacts: Contact Name, ID, ID Type, TS Override |
| TG_Lists.csv | TG lists: TG List Name, then Contact1 … Contact32 the contact names |
| APRS.csv | APRS configurations |

The files are UTF-8 (with a BOM, so Excel shows Chinese right), comma separated.

## Import

"Import CSV" takes the zip, or one or more CSV files at once (the files of a folder the OpenGD77 CPS exported, for
one). Some of them are enough, Channels.csv alone for one.

- The first row (the header) tells which file it is; columns are found by name (any case and spacing), so **their
  order does not matter, a missing column takes the default, an unknown one is left out**.
- Separator: comma, semicolon (Excel in some countries) or tab; frequencies with `.` or `,`.
- Encoding: UTF-8 or GBK (what Excel saves on a Chinese Windows).
- References go by **name**: a channel's Contact / TG List / APRS, a TG list's contacts, a zone's channels. A name
  not found is reported and left empty.
- With an error (a frequency outside the radio's bands, for one) nothing changes, and each error is listed with its
  file and line; warnings (a name cut to fit, for one) do not stop the import.
- DTMF.csv is not imported.

Two ways, as in the OpenGD77 CPS:

- **Replace** (OpenGD77's Import): each list in the files becomes the file's; the channel numbers are the Channel
  Number column (empty: in turn). The lists not imported stay; their references to a replaced list follow the names.
- **Append** (OpenGD77's Append): the channels are added after the ones there (the file's numbers left out); zones,
  contacts, TG lists and APRS configurations of the same name are replaced by the file's, the others added.

## Channels.csv columns

The first 28 are the OpenGD77 ones, the last 3 OpenDMR's.

| Column | Values |
| --- | --- |
| Channel Number | 1 – 1024 (Replace only) |
| Channel Name | 16 bytes at most (a Chinese character takes 2); characters without a GB2312 code show as ? |
| Channel Type | `Analogue` (FM), `Digital` (DMR); OpenDMR also `AM`, `USB`, `LSB`, `P25`, `NXDN`, `M17` |
| Rx Frequency | MHz, `439.46250` |
| Tx Frequency | MHz; empty: the Rx frequency |
| Bandwidth (kHz) | `12.5` or `25` (analogue) |
| Colour Code | 0 – 15 (DMR) |
| Timeslot | 1 or 2 (DMR) |
| Contact | a contact name, or `None` |
| TG List | a TG list name, or `None` |
| DMR ID | the channel's own DMR ID, or `None` (the radio's) |
| TS1_TA_Tx / TS2_TA_Tx ID | talker alias sent: `Off`, `APRS`, `Text`, `APRS+Text` |
| RX Tone / TX Tone | `None`, CTCSS `88.5`, DCS `D023N` / `D023I` |
| Squelch | `Disabled` (the radio's setting), `Open`, `0%` … `100%` (5 % steps), `Closed` |
| Power | `Master` (the radio's setting), `P1` … `P9`, `-W+`; or the watts: P1 = 50mW, P2 = 500mW, P3 = 1W, P4 = 2W, P5 = 3W, P6 = 4W, P7 = 5W, P8 = 7W, P9 = 10W |
| Rx Only, Zone Skip, All Skip | `Yes` / `No` |
| TOT | transmit time-out, s (15 s steps), 0: off |
| VOX | `On` / `Off` |
| No Beep, No Eco | `Yes` / `No` |
| APRS | an APRS configuration name, or `None` |
| Latitude, Longitude | the channel's place, decimal degrees (4 decimals), 0: none |
| Use Location | `Yes` / `No` (the place used to sort by distance) |
| NAC | **OpenDMR**: P25's NAC, 3 hex digits, empty: 293 (F7E: receive any NAC) |
| Step (kHz) | **OpenDMR**: the step, 2.5 / 5 / 6.25 / 10 / 12.5 / 25 / 30 / 50 |
| Force DMO | **OpenDMR**: DMR forced direct mode, `Yes` / `No` |
| RAN | **OpenDMR**: NXDN's RAN, 0 - 63, empty: 1 (0: receive any RAN) |
| CAN | **OpenDMR**: M17's channel access number, 0 - 15, empty: 0 (transmitted; receive: not filtered) |

`Yes / No` columns also take `True / False`, `1 / 0`.

## APRS.csv columns

| Column | Values |
| --- | --- |
| APRS config Name | 8 characters at most (8 configurations at most) |
| SSID | 0 – 15 |
| Via1, Via2 | the path, `WIDE1`, 6 characters at most |
| Via1 SSID, Via2 SSID | 0 – 15 |
| Icon table | 0: primary `/`, 1: alternate `\` |
| Icon | the symbol's ASCII code - 33 (`!` = 0) |
| Comment text | 24 characters at most (ASCII) |
| Ambiguity | position ambiguity 0 – 7 |
| Use position | `True`: the fixed place below |
| Latitude, Longitude | decimal degrees |
| TX Frequency | the beacon's transmit frequency in MHz, empty: the channel's |
| Transmit QSY | `True`: the beacon announces the channel's voice frequency |
| Baud rate setting | 0: 1200, 1: 300 |

## Example

A sheet with a few columns is enough:

```csv
Channel Name,Channel Type,Rx Frequency,Tx Frequency,RX Tone,TX Tone,Power
Repeater,Analogue,439.46250,434.46250,88.5,88.5,P7
Simplex 1,Analogue,145.50000,,None,None,Master
BM 4600,Digital,439.12500,431.12500,,,P9
```

Save it as CSV, then "Import CSV" → "Append".

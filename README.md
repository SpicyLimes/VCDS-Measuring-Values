# VCDS Measuring Values

Complete lists of the **Advanced Measuring Values** that VCDS offers for VW Group control modules, with their IDs (`IDE…`, `ENG…`, `MAS…`).

On newer UDS modules, VCDS has no numbered measuring blocks. You pick values from a long list instead, and that list isn't published anywhere. Searching for an ID like `IDE01975` online turns up nothing. This repo puts the full lists in one place, so you can see what a module offers, and what each ID is called, before you're sitting in the car.

## What's here

| Vehicle | Module | Part number (SW) | Rows | Files |
|---|---|---|---|---|
| 2018 VW Passat 2.0T (US) | 01 Engine | 06K 906 016 C | 817 | [TXT](VW/Passat-B8-NMS-2018-US-2.0T/01-Engine_06K906016C.txt) · [CSV](VW/Passat-B8-NMS-2018-US-2.0T/01-Engine_06K906016C.csv) |
| 2018 VW Passat 2.0T (US) | 02 Auto Trans | 09G 927 158 AB | 197 | [TXT](VW/Passat-B8-NMS-2018-US-2.0T/02-AutoTrans_09G927158AB.txt) · [CSV](VW/Passat-B8-NMS-2018-US-2.0T/02-AutoTrans_09G927158AB.csv) |

- **TXT:** the list exactly as VCDS shows it, in VCDS order, duplicates included. Easy to read and search.
- **CSV:** the same rows, plus columns that split each ID into its parts and a `notes` column with values seen in real logs. GitHub shows CSV files as a searchable table.

### Module details

| | 01 Engine | 02 Auto Trans |
|---|---|---|
| Controller | J623 | J217 |
| Software part no. | 06K 906 016 C | 09G 927 158 AB |
| Hardware part no. | 06K 907 309 | 09G 927 158 |
| Component | R4 2.0l TSI H30 9609 | AISIN AG6 G3 |
| VCDS label file | 06K-907-425-V2.clb | 09G-927-749-V2.clb |
| Engine / transmission | EA888 Gen 3B, engine code DDSA | 09G 6-speed automatic |

Read with VCDS 26.9.0, data version DS378.0.

## Will it match my car?

The IDs come from the module's own diagnostic data, not from the car.

- **Same software part number:** the list should match exactly.
- **Same VCDS label file, different part number:** most IDs should be the same. Other EA888 Gen 3 engines and 09G transmissions are likely close.
- **Different engine or module family:** some IDs will match and many won't. Use these lists as a guide to what kinds of values exist.

To check, find your module's part number and label file in an Auto-Scan. Then open Advanced Measuring Values and compare a few IDs.

## Reading the IDs

- **`IDEnnnnn`, `ENGnnnnn`, `MASnnnnn`** are the IDs VCDS shows in the "Loc." column. Ross-Tech describes them only as factory identifiers for data objects.
- **`IDE07224-ENG188621`:** the part after the dash is a sub-item of that ID. `-ENG188621` marks a "Bits 0-7" status byte.
- **Repeated rows are real.** VCDS lists some items several times.
- **"..." at the end of a description** means VCDS itself cuts the name off there.
- **Underscored names** like `Misfire_detection_sensor_wheel_adaptation_speed_load_range_index` are raw internal names. They have no translated label.

## Using an ID in VCDS

- **Find an item:** type the ID, or part of its name, into the Filter box at the top of the Advanced Measuring Values list.
- **Limits:** you can log up to 12 items at once. Each extra item lowers the sample rate. Tick **Turbo** for the fastest rate.
- **Save a selection:** click the VCDS logo at the top left of the item list and choose "Save selection to file". "Load selection from file" brings it back.
- **Build presets by hand:** a saved selection is a plain text file with one ID per line, sub-item suffix included. Engine selections end in `.u01` and transmission selections in `.u02`, because the number is the module address. You can write these files yourself from the lists here.
- **One module at a time:** VCDS logs one module per session. You can't log engine and transmission values side by side.

## Field notes

These values were confirmed in real VCDS logs on the car above. The CSV `notes` column has the rest.

| ID | What we saw |
|---|---|
| `IDE01775`–`01778` Misfires per 1000 revolutions | A held rate that updates at irregular intervals. Count changes from 0 to nonzero; don't average rows. |
| `IDE01975`–`01978` Numb. of misfir.: cylinder 1–4 | Running totals in the thousands. The difference between two readings is an exact misfire count. |
| `IDE06192`–`06195` Knock sensor: voltage | Knock sensor signal (V·ms) in the order cylinder 2, 3, 4, 1. Not timing retard. |
| `IDE00589` Fuel pressure | Direct-injection rail pressure in kPa, relative. About 6,000 at warm idle, up to about 22,000 under load. |
| `IDE00593` Ignition timing adjustment cylinder 1 | Degrees, −19 to +30. About −1 at warm idle; goes negative on throttle lift-off. |
| `IDE00738` Oxygen sensor test: delta lambda | A self-test result, not live lambda. |
| `IDE10041` Intake camshaft B1 active operating state | 8 = normal; 1 or 7 = hard acceleration; 6 = first ~2.5 min after a cold start. |
| `IDE02736` (02) Display of gear | 1–6 and R. Reads 1 in both P and N; use `IDE00096` for the selector position. |
| `IDE06672` (02) Current gear/clutch status | Gear plus a converter lock-up letter: M = locked, S = slipping, H or N = open. |

Not yet logged but likely useful: `IDE00155`–`00158` "Ignition timing angle retard cylinder 1–4" (per-cylinder knock retard) and `ENG119911` "UEGO r Lam S1 B1" (wideband lambda).

## How the lists were made

Each module's complete selection list was captured in screenshots (13 for the engine, 3 for the transmission) and transcribed row by row. No personal data is included.

If you spot a transcription error, please open an issue.

## Contributing

To add your own vehicle or module:

1. Open the module's Advanced Measuring Values, clear the Filter box, and screenshot the whole list.
2. Transcribe it in the same format, as both TXT and CSV.
3. Add a folder: `Make/Model-Generation-Year-Market-Engine/`.
4. Include the module details from an Auto-Scan: controller, software and hardware part numbers, component, label file and VCDS version.
5. **Remove your VIN, mileage and coding strings.**

## Disclaimer

This repo is not affiliated with Ross-Tech or Volkswagen AG. VCDS is a trademark of Ross-Tech, LLC. Descriptions appear as VCDS displays them. Everything here is for reading data only; nothing in this repo changes a module's coding or adaptation.

## License

The license covers only what this repo adds: the README, the formatting, the CSV structure and the field notes. Those are released under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) (public domain), so use them however you like.

The IDs and descriptions themselves are factual data, shown as VCDS displays them. They come from Volkswagen's diagnostic data and Ross-Tech's labels, and this repo claims no rights over them. No VCDS label files or software are included. If you represent Ross-Tech or Volkswagen and object to anything here, please open an issue and it will be removed.

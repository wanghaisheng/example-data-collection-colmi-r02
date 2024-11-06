# Colmi R02 raw data logging

This Python script, `ring.py`, logs raw sensor data from a [Colmi R02 ring](https://www.colmi.info/products/colmi-r02-smart-ring). It collects data such as accelerometer values, PPG (photoplethysmogram) readings, SpO2 (oxygen saturation) levels remotely using the Bluetooth data streaming. The script is designed to run on Python 3.11. It is recommended to updgrade the ring firmware for higher data streaming capabilities.

## Prerequisites

1. **Python Version**: Ensure you have Python 3.11 installed. Using `pyenv` is recommended for managing Python versions:
   ```bash
   pyenv install 3.11.0
   pyenv local 3.11.0
   ```

2. **Firmware Upgrade (optional but highly recommended)**:

    To support higher data streaming, it is highly recommended to upgrade the ring firmware. You can flash the new firmware using the following website: [ATC_RF03 Firmware Writer](https://atc1441.github.io/ATC_RF03_Writer.html)

    The firmware is located at the root of the repository: `R02_3.00.06_FasterRawValuesMOD.bin`

3. **Dependencies**: Install the required Python packages:
   ```bash
   pip install bleak pandas
   ```

## Usage

1. Clone this repository
2. Run the script with the following command:
    ```bash
    python ring.py --duration <duration_in_seconds>
    ```

*Notes: When running the script for the first time, it will ask you to select your ring. The address will be stored in a `config.json` file.*

```
> python ring.py --duration 60
0: SPEAKERS [E4095B2B-XXX-XXXX-XXXX-A5F4BEE8C7D8]
1: None [C7060F05-353C-E4DD-80FC-B635A1BCCCE8]
2: R02_2182 [BBF33765-XXXX-XXXX-XXXX-DD487BF7717C]
Select a device by entering its number: 2
```

If you don't see your device, make sure the ring is disconnected from previous connections (QRing app for example).

The script will:

1. Discover and connect to your Colmi R02 ring via Bluetooth.
2. Collect data from the ring and store it in CSV format in a `raw_data` folder.
3. Save the collected data to a CSV file, with a timestamped filename (e.g., `ring_data_YYYYMMDD_HHMMSS.csv`).

## Output Format

Each CSV file contains the following columns:

| Column       | Description                                                |
|--------------|------------------------------------------------------------|
| `timestamp`  | ISO 8601 formatted timestamp                               |
| `payload`    | Hexadecimal string representation of the raw data payload  |
| `accX`, `accY`, `accZ` | Accelerometer readings (X, Y, Z axes)          |
| `ppg_raw`, `ppg_max`, `ppg_min`, `ppg_diff` | PPG sensor readings         |
| `spO2_raw`, `spO2_max`, `spO2_min`, `spO2_diff` | SpO2 sensor data       |

### Example CSV Output

```csv
timestamp,payload,accX,accY,accZ,ppg_raw,ppg_max,ppg_min,ppg_diff,ppg,spO2_raw,spO2_max,spO2_min,spO2_diff
2024-11-06T13:45:22.087859,a10100a000c20072009501000000000c,,,,,,,,,160,194,114,149
2024-11-06T13:45:22.088225,a10229812acd29c1010c01000000003c,,,,10625,10957,10689,268,,,,,
2024-11-06T13:45:22.088273,a10312030b0d170400000000000000ec,372,291,-1859,,,,,,,,,
2024-11-06T13:45:22.328776,a101009900c200720095010000000005,,,,,,,,,153,194,114,149
2024-11-06T13:45:22.329075,a10229942acd29c1010c01000000004f,,,,10644,10957,10689,268,,,,,
2024-11-06T13:45:22.329126,a103110a0b01160800000000000000e9,360,282,-1871,,,,,,,,,
2024-11-06T13:45:22.567010,a101009900c200720095010000000005,,,,,,,,,153,194,114,149
2024-11-06T13:45:22.567513,a10229932acd29c1010c01000000004e,,,,10643,10957,10689,268,,,,,
2024-11-06T13:45:22.567588,a10312010c00160e00000000000000e7,366,289,-1856,,,,,,,,,
2024-11-06T13:45:22.806217,a101009900c200720095010000000005,,,,,,,,,153,194,114,149
2024-11-06T13:45:22.806648,a10229962acd29c1010c010000000051,,,,10646,10957,10689,268,,,,,
2024-11-06T13:45:22.806750,a10312010c00160e00000000000000e7,366,289,-1856,,,,,,,,,
2024-11-06T13:45:23.046104,a101009900c200720095010000000005,,,,,,,,,153,194,114,149
2024-11-06T13:45:23.046931,a10229972acd29c1010c010000000052,,,,10647,10957,10689,268,,,,,
2024-11-06T13:45:23.047054,a10312010c00160e00000000000000e7,366,289,-1856,,,,,,,,,
2024-11-06T13:45:23.286366,a101009900c200720095010000000005,,,,,,,,,153,194,114,149
2024-11-06T13:45:23.286761,a10229962acd29c1010c010000000051,,,,10646,10957,10689,268,,,,,
2024-11-06T13:45:23.287225,a10312010c00160e00000000000000e7,366,289,-1856,,,,,,,,,
2024-11-06T13:45:23.526652,a101009800c200720095010000000004,,,,,,,,,152,194,114,149
2024-11-06T13:45:23.527101,a10229942acd29c1010c01000000004f,,,,10644,10957,10689,268,,,,,
2024-11-06T13:45:23.527264,a10312010c00160e00000000000000e7,366,289,-1856,,,,,,,,,
2024-11-06T13:45:23.767238,a101000000c20072009501000000006c,,,,,,,,,0,194,114,149
2024-11-06T13:45:23.767739,a10200002acd29c1010c010000000092,,,,0,10957,10689,268,,,,,
2024-11-06T13:45:23.767887,a10312010c00160e00000000000000e7,366,289,-1856,,,,,,,,,
2024-11-06T13:45:24.007569,a101009800c200720095010000000004,,,,,,,,,152,194,114,149
2024-11-06T13:45:24.008782,a10229952acd29c1010c010000000050,,,,10645,10957,10689,268,,,,,
2024-11-06T13:45:24.008965,a10312010c00160e00000000000000e7,366,289,-1856,,,,,,,,,
2024-11-06T13:45:24.246474,a101009800c200720095010000000004,,,,,,,,,152,194,114,149
2024-11-06T13:45:24.247007,a10229932acd29c1010c01000000004e,,,,10643,10957,10689,268,,,,,
2024-11-06T13:45:24.247156,a10312010c00160e00000000000000e7,366,289,-1856,,,,,,,,,
```

## Next Steps

Parse the raw data to be ingested in [Edge Impulse Studio](studio.edgeimpulse.com).
This will probably require to interpolate missing values, normalize some values, etc...

## Resources

To build this script, I have used the following resources:

**OTA flasher:**

* [ATC RF03 Ring OTA Flasher](https://atc1441.github.io/ATC_RF03_Writer.html)

**Github Repositories:**

* [ATC_RF03_Ring](https://github.com/atc1441/)
* [colmi_r06_fbp](https://github.com/CitizenOneX/colmi_r06_fbp)
* [colmi_r02_client](https://github.com/tahnok/colmi_r02_client)

**Articles:**

* [2024-07-07 Smart Ring Hacking](https://notes.tahnok.ca/blog/2024-07-07+Smart+Ring+Hacking)
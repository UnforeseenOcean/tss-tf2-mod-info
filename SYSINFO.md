# Hardware info

- SoC: Qualcomm QCM4325
- RAM: 3GB DDR4
- ROM (eMMC): 32GB
- Actual manufacturer: SUN M I Technology
- Camera: 2x 5 Megapixel Autofocus Cameras

# Software info
- Base OS: Android 13
- Latest Known Build: 1.609.13
- Latest Known FW: 489
- Latest Known SV: 8.5.2.A
- PK_HASH: `0x206137a09c3b75287bec8ab3c1263df62f2206510effd225f9da8186bd805794`
- Required loader: Fire hose
- Toolkit Required: B.Kerler EDL
- OTA Type (APP): LiteOS Image / Binary Blob
- OTA Type (SYS): Binary Blob (Possibly Bzip2)
- Payload Structure:
  - `care_map.pb`: I don't know why it is called "care_map" but it is a map of where the keys go, seemingly
  - `apex_info.pb`: Empty for 1.609.5/475
  - `payload.bin`: Binary blob containing all the good stuff
  - `payload_properties.txt`: Contains file hash, size, metadata hash, and metadata size
  - `META-INF/com/android/otacert`: Public key of the OTA (private key is still unknown, which is required to sign the update)
  - `META-INF/com/android/metadata.pb`: Contains info about what this OTA package is for, where it should go, and various keys it will install
  - `META-INF/com/android/metadata`: Describes what OTA update files will be used for, and how it is made up
- TODO: Find 1.609.13 OTA update

## Guesses
- API UA filter: `okhttp/4.11.0` probably
- API Endpoint: `https:// placed - a pie. *********.com` (obscured)
- Requires more RE, but the findings will be obscured 

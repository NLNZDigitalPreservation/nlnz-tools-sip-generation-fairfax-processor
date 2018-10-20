# National Library of New Zealand Tools SIP Generation Processor for Fairfax files

Build script for Fairfax-based SIPs for ingestion into the Rosetta archive system.

## Synopsis

Builds Fairfax-based SIPs for ingestion into the Rosetta archive system.

## Motivation

We want to automate the generation of Fairfax SIPs from Fairfax files.

## Important

At this time there is no important information to impart.

## Requirements

- A set of Fairfax files for ingestion
- The nlnz-m11n-tools-automation-plugin
- The nlnz-m11n-tools-gradle-plugin
- The nlnz-tools-sip-generation-gradle-plugin
- The nlnz-tools-sip-generation-fairfax-gradle-plugin

## Usage

### Processing files
Process a set of Fairfax files.

Example:
```
gradle processFiles \
    -PsourceFolder="/path/to/source/folder" \
    -PdestinationFolder="/path/to/destination/folder"
```
### Parameters

Parameters and their usage.

#### sourceFolder
The source folder of the Fairfax files.
```
-PsourceFolder="/path/to/source/folder"
```

#### destinationFolder
The destination folder for processed files.
```
-PdestinationFolder="/path/to/destination/folder"
```

## Contributors

See git commits to see who contributors are. Issues are tracked through the git repository issue tracker.

## License

&copy; 2018 National Library of New Zealand. All rights reserved. MIT license.

# National Library of New Zealand Tools SIP Generation Processor for Fairfax files

Build script for Fairfax-based SIPs for ingestion into the Rosetta archive system.

## NO LONGER ACTIVE

*Note* that this project is no longer active, as the processing functionality has been moved to the
`sip-generation-fairfax-processor` project/subproject (see
https://github.com/NLNZDigitalPreservation/nlnz-tools-sip-generation-fairfax). Please see that project's `README.md`
for more information about Fairfax files processing. 

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

### Staged Processing

There are several stages to processing, each of which has its own section:

1. *groupByDateAndName: Grouping the files by date and name*.
2. *processByDate: Process the files by date*.

The `processFiles` task will perform the `groupByDateAndName` and `processByDate` tasks in sequence.

See the *Parameters* section for a discussion of the different parameters used.

### Other processing

#### listFiles: list files based on source folder
`listFiles` simply lists files by name, edition and date:
```
gradle listFiles \
    -PstartingDate="yyyyMMdd" \
    -PendingDate="yyyyMMdd" \
    -PgenericSourceFolder="/path/to/source/folder"
```

#### extractMetadata: extract metadata from the pdf files based on source folder
```
gradle extractMetadata \
    -PstartingDate="yyyyMMdd" \
    -PendingDate="yyyyMMdd" \
    -PgenericSourceFolder="/path/to/source/folder"
```

#### copyProdLoadToTestStructures: Copy production load files
Copies files from previous production loads into Rosetta into groupByDateAndName *and* pre-Rosetta ingest structures
for testing. The structures are as follows:
1. groupByDateAndName structure. This is to mimic the input to processByName.
   Directory structure: groupByDateAndName/<yyyyMMdd>/<name>/{files}
2. post-processByDate structure. This is the structure that gets ingested into Rosetta.
   Directory structure: rosettaIngest/<date-in-yyyMMdd>/<name>_<yyyyMMdd>/{files}

These structures provide for testing the Fairfax processor, to see if its outputs match the work done previously.

```
gradle copyProdLoadToTestStructures \
    -PstartingDate="yyyyMMdd" \
    -PendingDate="yyyyMMdd" \
    -PgenericSourceFolder="/path/to/source/folder" \
    -PgenericDestinationFolder="/path/to/destination/folder"
```

### groupByDateAndName: Grouping the files by date and name
The first stage of processing where files are separated out by date and name.
```
gradle groupByDateAndName \
    -PstartingDate="yyyyMMdd" \
    -PendingDate="yyyyMMdd" \
    -PgroupByDateAndNameSourceFolder="/path/to/source/folder" \
    -PgroupByDateAndNameDestinationFolder="/path/to/destination/folder" \
    -PgroupByDateAndNameMoveFiles="false" \
    -PgroupByDateAndNameCreateDestination="true"
```

### processByDate: Process the files by date
The second state of processing where files are aggregated into specific SIPs ready for ingestion into Rosetta.
```
gradle processByDate \
    -PstartingDate="yyyyMMdd" \
    -PendingDate="yyyyMMdd" \
    -PprocessByDateSourceFolder="/path/to/source/folder" \
    -PprocessByDateDestinationFolder="/path/to/destination/folder" \
    -PprocessByDateMoveFiles="false" \
    -PprocessByDateCreateDestination="true"
```

### Processing files
Does all stages of processing a set of Fairfax files. Currently those stages are `groupByDateAndName` and
`processByDate`.

Example:
```
gradle processFiles \
    -PstartingDate="yyyyMMdd" \
    -PendingDate="yyyyMMdd" \
    -PgroupByDateAndNameSourceFolder="/path/to/source/folder" \
    -PgroupByDateAndNameDestinationFolder="/path/to/destination/folder" \
    -PgroupByDateAndNameMoveFiles="false" \
    -PgroupByDateAndNameCreateDestination="true" \
    -PprocessByDateSourceFolder="/path/to/source/folder" \
    -PprocessByDateDestinationFolder="/path/to/destination/folder" \
    -PprocessByDateMoveFiles="false" \
    -PprocessByDateCreateDestination="true"
```

### Parameters

Parameters and their usage.

#### startingDate
The starting date for file processing (inclusive). Files before this date are ignored. Note that the `startingDate` is
based on the file name, and not the time stamp of the file. Files usually have the format:
```
<Name><Edition>-yyyyMMdd-<optional-sequence-letter><optional-sequence-number>
```

The format of the starting date is the same format as in the file, namely `yyyyMMdd`.

#### endingDate
The ending date for file processing (inclusive). Files after this date are ignored. Note that the `endingDate` is based
on the file name, and not the time stamp of the file. The format of the starting date is the same format as in the file,
namely `yyyyMMdd`.

#### groupByDateAndNameSourceFolder
The source folder for `groupByDateAndName` processing. This value must be specified when processing with
`groupByDateAndName`. The assumption is that all the source files are in the same directory (there are no
subdirectories).

#### groupByDateAndNameDestinationFolder
The destination folder for `groupByDateAndName` processing. This value must be specified when processing with
`groupByDateAndName`. Files that are processed by end up in this folder structure:
````
<groupByDateAndNameDestinationFolder>/
  |- <yyyyMMdd>/<name>/{files-for-the-given-name>
  |- UNKNOWN/<yyyyMMdd>/{files-that-have-no-name-mapping-for-that-date}
````

#### groupByDateAndNameMoveFiles
Whether files are moved or copied to the `groupByDateAndNameDestinationFolder` when processed. The default is `false`.

#### groupByDateAndNameCreateDestination
Whether the `groupByDateAndNameDestinationFolder` is created if it does not exist when using `groupByDateAndName`
processing. The default is `false`.

#### processByDateSourceFolder
The source folder for `processByDate` processing. This value must be specified when processing with `processByDate`. The
assumption is that all the source files are in the directory structure created by the `groupByDateAndName` processing.

#### processByDateDestinationFolder
The destination folder for `processByDate` processing. This value must be specified when processing with
`processByDate`. Files that are processed by end up in this folder structure:
````
<processByDateDestinationFolder>/
  |- <yyyyMMdd>/<ingest-key>/{files-for-the-given-ingest-key>
  |- UNKNOWN/<yyyyMMdd>/<name>/{files-that-have-no-ingest-key-for-that-date}
````

#### processByDateMoveFiles
Whether files are moved or copied to the `processByDateDestinationFolder` when processed. The default is `false`.

#### processByDateCreateDestination
Whether the `processByDateDestinationFolder` is created if it does not exist when using `processByDate` processing.
The default is `false`.

#### genericSourceFolder
The source folder for generic processing, including `listFiles` and `extractMetadata`. This value must be specified.
Depending on the processing done, the files may need to be in the same folder or may be contained in subdirectories.

## Contributors

See git commits to see who contributors are. Issues are tracked through the git repository issue tracker.

## License

&copy; 2018 &mdash; 2019 National Library of New Zealand. All rights reserved. MIT license.

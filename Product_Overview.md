# Product Overview

The HIEv / DIVER software provides a means for capture and long term storage of data and meta-data from various instruments. The software is not specific to any instruments, but does do special processing on TOA5 files from Campbell Scientific Loggers. It is designed to provide rich functionality around time-series data, as well as the ability to store and organise other types of files.

Initially, the software was developed to manage data from the following three sites:
- Eddy Flux Tower – which collects meteorological and flux data from within an isolated forest environment.
- Whole tree chambers – which collect meteorological data of the environmental impact on a tree wholly encapsulated within the chamber.
- Weather station – which collects meteorological data where positioned.  
 As of November 2015, this has been expanded to 9 sites inculding data from the HIE EucFace site, Glasshouse data and longitudinal field-based image capture of flora.

## Key features are:
- API to accept automatic ingest from datalogger PCs.
- API to allow programatic creation and publishing of packages.
- Manual upload for processed/auxiliary data.
- Automatic detection and handling of overlapping time-series data.
- Automatic extraction of metadata from time-series files.
- Management of information about experiments and facilities, and linking them to uploaded data.
- Advanced search, including search for time-series data based on dates.
- Custom download for time-series data which trims time-series files down to requested dates.
- Data packaging and publishing capability.
- RIF-CS feed with accompanying data zip bundle for harvesting by UWS library and eventual delivery to Research Data Australia.
- User management and provisioning plus permissions to control access to data.
- Support for data uploaded from Campbell Scientific data loggers in TOA5 format.
- Support for NETCDF and NCML file formts.
- Extraction of EXIF data from image files.
- Auto-detection of file MIME types.
- Auto OCR and speech recognition functions.

## Problem statement:

#### The problem of
short and long-term management of environmental datasets

#### affects
technical officers who manage the equipment and researchers who use the data.

#### the impact of which is:
- data is hard to find.
- a lot of manual work is involved in preparing the data for use.
- data may not be able to be reused in the future.
- sharing with external researchers is not simple.

#### A successful solution would:
- ensure raw data is never lost.
- ensure that data can be used and interpreted in the future.
- allow researchers to make linkages between different types of data.
- make it easier to researchers to access and query to the data (catering to both advanced and casual users).
- make it easier for technical officers to distribute the data.
- be flexible enough to handle changes such as new sensors being added.

## Overview Diagram
![Overview Diagram](files/overview-diagram.png)

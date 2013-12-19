# Product Overview

The DC21 HIE Climate Change and Energy Research Data Capture Project supports the Cell to Ecosystem-focused Climate Change and Energy Research Facilities (CCERF) at the UWS Hawkesbury campus.

The software provides a means for capture and long term storage of data and meta-data from various instruments. The software is not specific to any instruments, but does do special processing on TOA5 files from Campbell Scientific Loggers. It is designed to provide rich functionality around time-series data, as well as the ability to store and organise other types of files.

Initially, the software will be used to manage data from the following sites:
- Eddy Flux Tower – which collects meteorological and flux data from within an isolated forest environment.
- Whole tree chambers – which collect meteorological data of the environmental impact on a tree wholly encapsulated within the chamber.
- Weather station – which collects meteorological data where positioned.

## Key features are:
- API to accept automatic ingest from datalogger PCs.
- Manual upload for processed/auxiliary data.
- Automatic detection and handling of overlapping time-series data.
- Automatic extraction of metadata from time-series files.
- Management of information about experiments and facilities, and linking them to uploaded data.
- Advanced search, including search for time-series data based on dates.
- Custom download for time-series data which trims time-series files down to requested dates.
- RIF-CS feed with accompanying data zip bundle for harvesting by UWS library and eventual delivery to Research Data Australia.
- User management and provisioning plus permissions to control access to data.

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
![Overview Diagram](https://github.com/IntersectAustralia/dc21/raw/master/doc/overview-diagram.png)

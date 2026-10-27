---
title: "Another Weather Station - Oasi"
date: 2026-10-26T21:35:00+01:00
description: "Building a personal weather station with 3D printed sensor enclosures and a Go microservice backend for forecasting."
image: /images/posts/2026-10-26-weather-station/stevenson-screen.webp
tags:
  - Weather Station
  - 3D Printing
  - IoT
  - Golang
  - Python
  - Time-series forecasting
  - Timestral
  - Mistral
  - Uber Fx
---

I have wanted to build a personal weather station to collect local data and experiment with weather forecasting for a long time. The complete source code and design files are available on [GitHub](https://github.com/jotonedev/weather-station).
The codename for this project is "Oasi".

## System architecture

I wanted to challenge myself with a microservice architecture because I had never used one in personal projects.

![System architecture](/images/posts/2026-10-26-weather-station/architecture.png)

The infrastructure runs across two environments. Most services run on premises on a k3s cluster built from Raspberry Pi boards. The inference workload requires more compute, so it runs on an Oracle Cloud virtual machine with two ARM cores and 12 GB of memory.

I considered dedicated time series engines like InfluxDB and QuestDB for storage. Instead, I selected DuckDB to keep the footprint lightweight for the local cluster.

## Services and implementation

I chose Go wherever possible together with [Uber Fx](https://uber-go.github.io/fx/) because I am accustomed to working with both. The backend services communicate over gRPC.

For the inference service, I went with Python because the vast majority of AI and forecasting libraries target the Python ecosystem. I considered translating an inference implementation to Rust with AI assistance, but I preferred starting with a battle-tested foundation.

To build the services, I used a mix of self-hosted models and Antigravity.

Because the physical station is still being assembled, I validated the entire backend with a synthetic test data generator. The generator simulates real weather patterns and submits telemetry to the ingest endpoint, confirming that ingestion, database persistence and forecast jobs work as expected.

## Weather prediction

I use pre-trained time series foundation models to generate probabilistic weather forecasts. The inference server defaults to Mistral Timestral 209M, with Amazon Chronos 2 supported as an alternative backend.

### Hourly and daily horizons

The pipeline generates forecasts on two distinct time scales.

For the hourly forecast, an automated job retrieves up to 40 days of hourly observations from DuckDB. The model generates a 24-hour horizon forecast covering temperature, relative humidity, rainfall depth and precipitation confidence for each upcoming hour.

For the daily forecast, the job queries up to a full year of daily climate aggregates. The model then predicts weather conditions across a 14-day horizon, forecasting daily minimum and maximum temperature, expected humidity ranges and total rainfall.

### Input variables and features

Raw sensor telemetry passes through a preprocessing pipeline before reaching the model. The pipeline aligns the data into regular time buckets, fills short sensor dropouts and enriches the input with extra meteorological features.

- Direct station observations cover mean, minimum and maximum temperature, relative humidity, atmospheric pressure, wind speed, wind direction and accumulated rainfall.
- Derived meteorological metrics include the dew point calculated with the Magnus-Tetens formula, the diurnal temperature range, pressure tendency and wind vectors split into eastward and northward components.
- Cyclical calendar and astronomical covariates include sine and cosine transformations of the hour and day of the year, alongside solar declination and daylight fraction derived from station coordinates.

Feeding astronomical solar curves alongside physical pressure and humidity trends gives the zero-shot model the context needed to predict diurnal heating cycles and seasonal changes.

In the future, I plan to explore fine-tuning the models on local history using LoRA, as well as applying quantization to reduce memory usage.

## Weather station

### Hardware setup and wireless link

The station divides responsibilities between two ESP32-S3 boards.

Outdoors at the sensor mast, the control node samples all weather sensors once every minute. After collecting a reading, the firmware packs the measurements into a binary frame and transmits it over a 433 MHz radio link using a CC1101 transceiver. The custom ARQ protocol handles acknowledgments and retries to prevent packet loss.

Indoors, a LOLIN S3 Mini acts as the base station. It listens for radio packets, verifies frame integrity and returns acknowledgments to the mast. Each valid reading is stamped with the current time through NTP and queued in memory. An asynchronous task then batches the queued readings and sends them to the ingest microservice over Wi-Fi with exponential backoff. Keeping the queue in memory prevents flash wear while preserving readings across reboots and network dropouts.

Looking ahead, I would like to replace the CC1101 transceivers with LoRA modules to gain better transmission range and obstacle penetration. I also plan to update the base station firmware to coordinate multiple weather stations at the same time. On the sensor side, adding ambient luminosity and cloud cover sensors would provide extra context for weather forecasting.

### 3D models and enclosures

I printed all parts on an Elegoo Neptune 4 Plus with Elegoo PETG Pro filament.

The enclosures are remixes of existing designs modified for my setup. I updated the models to pass unterminated Cat5e network cables directly from each sensor enclosure back to the control node. I also adapted the rain gauge, anemometer and wind vane to use AS5600 magnetic rotary encoders. Replacing mechanical switches with AS5600 sensors improves reliability and reduces costs.

The remixes and original source projects are listed below.

- [Rain gauge remix on NexPrint](https://www.nexprint.com/models/G9544172), remixed from the [SS4H-RG rain gauge](https://smartsolutions4home.com/ss4h-rg-rain-gauge/)
- [Stevenson screen remix on NexPrint](https://www.nexprint.com/models/G4544173), remixed from the [Airrohr Stevenson screen on Printables](https://www.printables.com/model/1007462-sensor-community-airrohr-stevenson-screen)
- [Anemometer and wind vane remix on NexPrint](https://www.nexprint.com/models/G1210845), remixed from the [MakerWorld project](https://makerworld.com/en/models/2154241-anemometer-wind-vane) and [GitHub repository](https://github.com/slash4you/Anemometre-Girouette)

### Enclosure photos

![Stevenson screen air sensor box](/images/posts/2026-10-26-weather-station/stevenson-screen.webp)

![Anemometer and wind vane assembly](/images/posts/2026-10-26-weather-station/anemometer-windvane.webp)

![Tipping-bucket rain gauge](/images/posts/2026-10-26-weather-station/rain-gauge.webp)

## Current status

With the backend verified by synthetic telemetry, the remaining milestone is field deployment. I still need to wire the three sensor enclosures to the control node and mount them outside. Once the physical hardware is installed and collecting real readings, I will post another update.

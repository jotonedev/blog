---
title: "Another Weather Station"
date: 2026-10-26T21:35:00+01:00
image: /images/posts/2026-10-26-weather-station/stevenson-screen.webp
---

I have wanted to build a personal weather station to collect local data and experiment with weather forecasting for a long time. The complete source code and design files are available on [GitHub](https://github.com/jotonedev/weather-station).

## System architecture

I wanted to challenge myself with a microservice architecture because I had never used one in personal projects.

![System architecture](/images/posts/2026-10-26-weather-station/architecture.png)

The infrastructure runs across two environments. Most services run on premises on a k3s cluster built from Raspberry Pi boards. The inference workload requires more compute, so it runs on an Oracle Cloud virtual machine with two ARM cores and 12 GB of memory.

I considered dedicated time series engines like InfluxDB and QuestDB for storage. Instead, I selected DuckDB to keep the footprint lightweight for the local cluster.

## Services and implementation

I chose Go wherever possible together with [Uber Fx](https://uber-go.github.io/fx/) because I am accustomed to working with both. The backend services communicate over gRPC.

For the inference service, I went with Python because the vast majority of AI and forecasting libraries target the Python ecosystem. I considered translating an inference implementation to Rust with AI assistance, but I preferred starting with a battle-tested foundation. In the future, I might still explore fine-tuning the Timestral or Chronos-2 models with LoRA, as well as adding quantization.

To build the services, I used a mix of self-hosted models and Antigravity.

Because the physical station is still being assembled, I validated the entire backend with a synthetic test data generator. The generator simulates real weather patterns and submits telemetry to the ingest endpoint, confirming that ingestion, database persistence and forecast jobs work as expected.

## Weather station

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

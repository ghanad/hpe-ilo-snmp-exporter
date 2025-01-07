# HPE iLO SNMP Exporter

SNMP Exporter configuration for HPE iLO servers - monitoring hardware status, power supplies, fans, temperature sensors and storage components via SNMP.

## Overview

This repository contains SNMP exporter configuration for monitoring HPE servers through their iLO interface. It enables comprehensive hardware monitoring including:

- System overall health
- Power supplies status and metrics
- Fan status and speeds
- Temperature sensors
- Memory modules status
- Storage components (physical drives, logical drives, array controllers)
- Network interfaces status

## Prerequisites

- Prometheus SNMP exporter
- HPE servers with iLO interface configured
- SNMP v1/v2c enabled on iLO

## Usage

There are two ways to use this configuration:

### Method 1: Copy the HPE module to existing configuration
1. Open your existing SNMP exporter configuration file (usually `snmp.yml`)
2. Copy the `hpe` module section from this repository's `snmp.yml`
3. Paste it under the `modules` section in your existing configuration
4. Restart SNMP exporter to apply changes

### Method 2: Use as standalone configuration
1. Download the `snmp.yml` file
2. Place it in your SNMP exporter configuration directory
3. Update your SNMP exporter to use this configuration file
4. Restart SNMP exporter to apply changes

Note: Method 1 is recommended if you already have SNMP exporter configured for other devices, as it allows you to maintain a single configuration file.

Example Prometheus configuration:

```yaml
scrape_configs:
  - job_name: 'hpe_ilo'
    static_configs:
      - targets:
        - your-ilo-host  # SNMP device.
    metrics_path: /snmp
    params:
      module: [hpe]
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: localhost:9116  # SNMP exporter.
```

## Monitored Metrics

### System Metrics
- System uptime
- Product name
- System name
- Overall health status
- iLO firmware version
- Power management status

### Hardware Components
- Fan status and presence
- Temperature readings and thresholds
- Power supply status, capacity, and voltage
- Memory module status and size
- Network interface status
- Physical drive status and details
- Logical drive status and configuration

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- Based on HPE iLO SNMP reference
- Built for Prometheus SNMP exporter

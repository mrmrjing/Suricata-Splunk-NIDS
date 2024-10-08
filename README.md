# Suricata-Splunk Network Intrusion Detection System

This project integrates Suricata, an open-source Network Intrusion Detection System (NIDS), with Splunk, the powerful data collection and visualization tool. It aims to provide a robust solution for real-time monitoring of network traffic, analyzing security events, and alerting on potential threats.

## Features

- **Real-Time Monitoring**: Utilise Suricata's capabilities to monitor network traffic in real-time.
- **Log Analysis**: Use Splunk to analyze logs generated by Suricata for deep insights into network activities.
- **Alerts and Dashboards**: Setup custom alerts and dashboards in Splunk for effective threat management and visualization.

## Project Structure

- `configs/`: Contains configuration files for the Splunk Universal Forwarder.
  - `inputs.conf`: Specifies which logs to monitor.
  - `outputs.conf`: Defines where to send the collected logs.

## Prerequisites

- Suricata installed and configured to output logs in JSON format to `/var/log/suricata/eve.json`.
- Splunk Enterprise set up to receive data on port 9997, and the `suricata_logs` index should be created to store the logs.

## Installation and Setup

### Installing Suricata 
1. **On Ubuntu/Debian**: 
```bash 
sudo apt-get install 
sudo apt-get install suricata
```

2. **Update Suricata Rules**:
Suricata uses rules to identify potential threats 
```bash
sudo suricata-update
```

3. **Configure Suricata**:
Edit the configuration file to adjust settings such as network interface and rule paths 
```bash 
sudo nano /etc/suricata/suricata.yaml
```
- Modify the HOME_NET variable to your local network, e.g., 192.168.1.0/24.
- Set the network interface under the af-packet section that Suricata should monitor. Use `ctrl+w` to search for `af-packet`.

4. To test the configuration to make sure everything is set up correctly:
```bash
sudo suricata -T -c /etc/suricata/suricata.yaml
```
    
5. **Run Suricata in IDS**: 
```bash
sudo suricata -c /etc/suricata/suricata.yaml -i wlan0
``` 
Replace `eth0` with the network interface you want to monitor 

6. **Monitor logs**: 
Check the logs such as `fast.log` to see the detected network events.
```bash
tail -f /var/log/suricata/fast.log
```

### Configuring the Splunk Forwarder

1. **Place Configuration Files**:
Copy `inputs.conf` and `outputs.conf` into your Splunk Forwarder's `$SPLUNK_HOME/etc/system/local/` directory.
```bash
/opt/splunkforwarder/etc/system/local
```

2. **Modify `outputs.conf`**:
Replace `<indexer_ip>` with the IP address of your Splunk indexer.

3. **Restart Splunk Forwarder**:
Apply changes by restarting the Splunk Forwarder:
```bash
sudo $SPLUNK_HOME/bin/splunk restart
```

### Creating the Splunk Index
1. **Access Splunk Web Interface**:
Open the Splunk interface (http://<splunk_ip>:8000).

2. **Navigate to Indexes Management**:
Go to Settings > Indexes > New Index.

3. **Create Index**:
Name the index suricata_logs and configure settings as necessary.

### Verifying the configuration 
Ensure that logs are arriving in Splunk by searching the `suricata_logs` index: 
`index=suricata_logs`

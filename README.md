# 🛡️ DNS Threat Detector

An automated Threat Intelligence pipeline that monitors **Pi-hole** traffic, cross-references domains against **VirusTotal** and **AlienVault OTX**, automatically blocks malicious actors, and visualizes everything in **Grafana**.

---

## 🚀 Overview
This project creates a closed-loop security system for your home network:
1.  **Extract:** n8n pulls logs from your **Pi-hole** every 5 minutes.
2.  **Analyze:** Domains are checked against **AlienVault OTX** and **VirusTotal**.
3.  **Act:** If a domain exceeds a threat threshold (e.g., >= 10 malicious detections), n8n sends a command to Pi-hole to **immediately block** it.
4.  **Log:** All scan results are stored in **InfluxDB**.
5.  **Visualize:** A **Grafana** dashboard displays real-time threat levels and block history.

## 🛠️ Prerequisites
* **Pi-hole** (with API access enabled)
* **n8n** (Self-hosted or Cloud)
* **InfluxDB 2.x** (For data storage)
* **Grafana** (For the dashboard)
* **API Keys:**
    * [AlienVault OTX](https://otx.alienvault.com/) (Free)
    * [VirusTotal](https://www.virustotal.com/) (Free tier)

---

## 📦 Installation

### 1. n8n Workflow
1.  Open your n8n instance and create a **New Workflow**.
2.  Open the `n8n_workflow.json` file from this repo.
3.  Copy the content and paste it directly onto the n8n canvas.
4.  **Configuration Required:**
    * Update the **HTTP Request** nodes with your Pi-hole and InfluxDB IP addresses.
    * Enter your API keys in the Header parameters for the **VirusTotal** and **AlienVault** nodes.
    * Update the **Push to InfluxDB** node with your Org and Bucket names.

### 2. InfluxDB Setup
Create a bucket (e.g., `DNS_Security`) in your InfluxDB UI. Ensure your API token has **Write** permissions for this bucket.

### 3. Grafana Dashboard
1.  In Grafana, go to **Dashboards** > **New** > **Import**.
2.  Upload the `grafana_dashboard.json` file or paste the JSON text.
3. The JSON contains the placeholder `[YOUR_BUCKET_NAME]`. You should find and replace this with your actual InfluxDB bucket name before importing, or update the queries in the panels manually after the import is complete.

---

## ⚙️ Configuration Variables

To get the workflow and dashboard running, you must replace the following placeholders within the JSON files with your specific environment details (dont forget to remove the "[]":

| Placeholder | Description | Where to find it |
| :--- | :--- | :--- |
| `[YOUR_PIHOLE_IP]` | The local IP address of your Pi-hole instance. | `hostname -I` on your Pi-hole |
| `[YOUR_PIHOLE_AUTH_TOKEN]` | Your Pi-hole Web API password hash. | `/etc/pihole/setupVars.conf` |
| `[YOUR_INFLUXDB_IP]` | The local IP where InfluxDB 2.x is hosted. | Your server/container IP |
| `[YOUR_INFLUX_TOKEN]` | Your InfluxDB API Write/Read Token. | InfluxDB UI > API Tokens |
| `[YOUR_ORG_NAME]` | Your InfluxDB Organization name. | InfluxDB UI > About/Org |
| `[YOUR_BUCKET_NAME]` | The name of the bucket used for storage. | InfluxDB UI > Buckets |
| `[YOUR_VIRUSTOTAL_API_KEY]` | Your personal VirusTotal API Key. | [VT User Settings](https://www.virustotal.com/) |
| `[YOUR_ALIENVAULT_API_KEY]` | Your personal AlienVault OTX API Key. | [OTX Settings](https://otx.alienvault.com/) |

---

## 🤝 Contributing
Feel free to fork this repository and submit pull requests. For major changes, please open an issue first to discuss what you would like to change.

## 📄 License
[MIT](https://choosealicense.com/licenses/mit/)

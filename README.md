# DFIR Platform - Cortex Analyzers

Cortex analyzers for the [DFIR Platform](https://platform.dfir-lab.ch) API by [DFIR Lab](https://dfir-lab.ch). These analyzers integrate DFIR Platform's threat intelligence and analysis capabilities directly into [TheHive](https://thehive-project.org/) and [Cortex](https://github.com/TheHive-Project/Cortex).

## Analyzers

### DFIRPlatform_IOCEnrichment

Enrich indicators of compromise using 14+ intelligence sources.

- **Data types:** IP, domain, hash, URL
- **API endpoint:** `POST /v1/ioc/enrich`
- **Sources:** VirusTotal, AbuseIPDB, Shodan, OTX, GreyNoise, URLhaus, and more

### DFIRPlatform_PhishingAnalysis

Analyze phishing emails with 26+ analysis modules.

- **Data types:** file (EML)
- **API endpoint:** `POST /v1/phishing/analyze`
- **Modules:** SPF/DKIM/DMARC validation, header anomaly detection, URL reputation, attachment scanning, brand impersonation detection, and more

### DFIRPlatform_ExposureScan

Scan a domain's external attack surface using 11 providers.

- **Data types:** domain
- **API endpoint:** `POST /v1/exposure/scan`
- **Providers:** Shodan, Criminal IP, SSL Labs, crt.sh, SecurityTrails, and more

## Prerequisites

- [Cortex](https://github.com/TheHive-Project/Cortex) 3.x or later
- Python 3.8+
- A DFIR Platform API key (sign up at [platform.dfir-lab.ch](https://platform.dfir-lab.ch))

## Installation

### Option 1: Copy to Cortex analyzers directory

```bash
# Clone this repository
git clone https://github.com/dfir-lab/dfir-cortex-analyzers.git

# Copy analyzers to your Cortex analyzers path
cp -r dfir-cortex-analyzers/analyzers/DFIRPlatform_* /opt/Cortex-Analyzers/analyzers/

# Install dependencies for each analyzer
for dir in /opt/Cortex-Analyzers/analyzers/DFIRPlatform_*/; do
    pip3 install -r "${dir}requirements.txt"
done
```

### Option 2: Add as additional analyzer path

Add the path to your `application.conf`:

```
analyzer {
  paths = [
    "/opt/Cortex-Analyzers/analyzers",
    "/path/to/dfir-cortex-analyzers/analyzers"
  ]
}
```

Then install dependencies:

```bash
pip3 install cortexutils requests
```

## Configuration

1. Log into your Cortex instance
2. Go to **Organization** > **Analyzers**
3. Find the **DFIRPlatform** analyzers and click **Enable**
4. Configure the following parameters:

| Parameter  | Required | Description                                      |
|------------|----------|--------------------------------------------------|
| `api_key`  | Yes      | Your DFIR Platform API key                       |
| `base_url` | No       | API base URL (default: `https://api.dfir-lab.ch/v1`) |

### Getting an API Key

1. Sign up at [platform.dfir-lab.ch](https://platform.dfir-lab.ch)
2. Navigate to **Settings** > **API Keys**
3. Generate a new API key
4. Copy the key into the Cortex analyzer configuration

## Usage

Once configured, the analyzers are available in TheHive and Cortex:

- **IOC Enrichment:** Right-click any observable (IP, domain, hash, URL) in TheHive and select the DFIRPlatform_IOCEnrichment analyzer
- **Phishing Analysis:** Upload an EML file as an observable and run DFIRPlatform_PhishingAnalysis
- **Exposure Scan:** Run DFIRPlatform_ExposureScan on any domain observable

### Taxonomy Levels

The analyzers return Cortex taxonomies with standard levels:

| Level       | Meaning                              |
|-------------|--------------------------------------|
| `safe`      | No threats detected                  |
| `info`      | Informational, no risk indicators    |
| `suspicious`| Potential risk, warrants review      |
| `malicious` | Confirmed threat or high risk        |

## Credits System

DFIR Platform uses a credit-based system:

- **Free tier:** 100 credits per month
- Each API call consumes credits based on the operation type
- Monitor your usage at [platform.dfir-lab.ch](https://platform.dfir-lab.ch)
- When credits are exhausted, the API returns HTTP 402 and the analyzer reports an error

## License

MIT License. See [LICENSE](LICENSE) for details.

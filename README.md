# Cisco DNA Center integration with Cisco PSIRT API

The purpose of this application is to generate a CSV file containing all the vulnerabilities that affect the devices that are part of a Cisco DNA Center managed network.

* Technology stack: Python3.6 (standalone, not part of any module)
* Status: Alpha

## Installation

In this implementation of the application, I've included the Cisco PSIRT database of vulnerabilities as of June-05-2019 in the vulnerabilities.json file in the repo.

Ideally you would create an account with https://apiconsole.cisco.com and register your application with the Cisco PSIRT API (for details on how to do this check out: https://developer.cisco.com/psirt) and retrieve the list of pertinent vulnerabilities to your infrastructure in live fashion. You could take advantage in this situation of the openVulnAPI (https://github.com/CiscoPSIRT/openVulnAPI) that Cisco has developed to make it easier to interact and retrieve Cisco PSIRT information in a programmable way.

Or you could follow my example and download the whole list of vulnerabilities on a weekly basis and save them to a file and use the application without a live interaction with the Cisco PSIRT API.

After you clone the repo locally on your machine, the main application is contained in the `01_vulnerability_scan.py` file. Make sure you use a Python3.6 virtual environment and pip install the requirements from the `requirements.txt` file.

After that edit the `dnac_config.py` file to point the application to your own Cisco DNA Center instance. I have already included the details of the DevNet DNA Center always-on sandbox for testing purposes:

`DNAC=os.environ.get('DNAC','sandboxdnac.cisco.com')
DNAC_PORT=os.environ.get('DNAC_PORT',443)
DNAC_USER=os.environ.get('DNAC_USER','devnetuser')
DNAC_PASSWORD=os.environ.get('DNAC_PASSWORD','Cisco123!')`

`vulnerability.json` will be used to store all the vulnerabilities affecting the Cisco DNA Center managed network in JSON format and as input to the CSV writer function in the main application.

## Usage

You would run the application with: `python 01_vulnerability_scan.py` and the `vulnerability.csv` file will contain a list of all the vulnerabilities affecting your infrastructure. If you would like to use a reservable Cisco DNA Center sandbox, you can find more details on [DevNet Sandbox](https://developer.cisco.com/sandbox/).

The rows `publication_url` and `sir` might be especially helpful in the generated CSV report as they contain the link to the actual PSIRT report of the specific vulnerability and the criticality of the vulnerability.

## How to test the software

The application was developed with:
* Cisco DNA Center version 1.2.10
* Python 3.6
* Cisco PSIRT API data formats current to June-05-2019.

No additional instructions should be needed besides the ones provided above in the `Installation` section. However any API changes in future versions of Cisco DNA Center or Cisco PSIRT API might impact the functionality of this application.

## Known issues

The application is currently permissive, in the sense that it checks the Cisco PSIRT vulnerabilities database for either the product name or the software version running on the network devices so the output CSV file might contain a small number of false positives. This represents a point in the application that can be improved, by better parsing and taking into account different parameters (date the vulnerability was discovered, criticality, etc.) when generating the CSV report.

## Getting help

If you have any questions, concerns, bug reports, etc., please DM me on twitter @aidevnet.
[![Run on Repl.it](https://repl.it/badge/github/CiscoDevNet/Cisco-DNA-Center-with-Cisco-PSIRT-API-integration)](https://repl.it/github/CiscoDevNet/Cisco-DNA-Center-with-Cisco-PSIRT-API-integration)
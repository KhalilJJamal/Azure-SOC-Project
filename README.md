# Azure SOC Honeynet Project

This project demonstrates the implementation of a honeynet within an Azure Security Operations Center (SOC) environment. The focus was on practicing incident response and capturing metrics before and after deploying NIST 800-53 standards.

## Table of Contents

- [Introduction](#introduction)
- [Installation](#installation)
- [Usage](#usage)
- [Methodology](#methodology)
- [Results](#results)
- [Contributing](#contributing)
- [License](#license)
- [Acknowledgements](#acknowledgements)

## Introduction

This project aims to enhance the incident response capabilities of an Azure SOC by deploying a honeynet and adhering to NIST 800-53 security controls. The project outlines the steps taken to set up the honeynet, the methodology behind the deployment of NIST 800-53 standards, and the impact of these standards on the SOC's ability to detect and respond to threats.

## Installation

To set up the honeynet in your Azure environment, follow these steps:

```bash
# Replace 'yourResourceGroup' with the name of your Azure resource group
az deployment group create --resource-group yourResourceGroup --template-file ./azuredeploy.json

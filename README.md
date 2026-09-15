# Purchase Order Processor

## Overview

Event-driven Azure integration solution for processing Purchase Orders
using Azure Logic Apps and Azure Service Bus.

## Architecture

Source System
      ↓
Azure Service Bus
      ↓
Azure Logic App
      ↓
Purchase Order Processing
      ↓
SAP / Salesforce

## Azure Services

- Azure Logic Apps
- Azure Service Bus
- Azure API Management
- Azure Resource Manager / ARM Templates

## Repository Contents

- RG_APIM/template.json
- RG_APIM/parameters.example.json
- LogicApp/Receive_PO.json

## Key Integration Concepts

- Event-driven architecture
- Asynchronous messaging
- Azure integration patterns
- API integration
- Error handling
- Infrastructure as Code

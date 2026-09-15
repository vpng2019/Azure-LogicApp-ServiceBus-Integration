# Purchase Order Processor

## Overview

Designed and implemented an event-driven integration pattern using Azure Logic Apps and Azure Service Bus. The Logic App publishes Purchase Order (PO) data to a Service Bus Topic. The topic has two independent subscribers, one for SAP and one for Salesforce.

This Publisher → Topic → Subscribers architecture enables SAP and Salesforce to consume and process the same PO data independently without creating any dependency between the systems. This improves decoupling, scalability and maintainability, while allowing each system to process messages according to its own requirements.

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
- Azure Resource Manager / ARM Templates

## Repository Contents

- template.json
- parameters.json
- LogicApp: Receive_PO.json

## Key Integration Concepts

- Event-driven architecture
- Asynchronous messaging
- Azure integration patterns
- Error handling
- Infrastructure as Code

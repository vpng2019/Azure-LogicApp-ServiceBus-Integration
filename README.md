# Azure-LogicApp-ServiceBus-Integration

## Overview

Designed and implemented an event-driven integration pattern using Azure Logic Apps and Azure Service Bus. The Logic App publishes Purchase Order (PO) data to an Azure Service Bus Topic. The Topic has two independent subscribers, one for SAP and one for Salesforce.

This Publisher → Topic → Subscribers architecture enables SAP and Salesforce to consume and process the same PO data independently without creating any dependency between the systems. This improves decoupling, scalability and maintainability, while allowing each system to process messages according to its own requirements.

## Architecture

Source System
      ↓
Azure Logic App
      ↓
Azure Service Bus Topic
      ↓
┌───────────────┬────────────────────┐
↓               ↓
SAP Subscriber  Salesforce Subscriber

## Azure Services

- Azure Logic Apps
- Azure Service Bus
- Azure Resource Manager / ARM Templates

## Repository Contents

- template.json
- parameters.json
- Receive_PO.json

## Key Integration Concepts

- Event-driven architecture
- Publisher → Topic → Subscribers pattern
- Asynchronous messaging
- Decoupled system integration
- Azure integration patterns
- Independent message consumption
- Scalability and maintainability
- Error handling
- Infrastructure as Code

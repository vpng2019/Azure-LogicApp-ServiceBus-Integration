# Azure-LogicApp-ServiceBus-Integration

## Overview

**Azure-LogicApp-ServiceBus-Integration** is an event driven Azure integration solution designed to demonstrate how **Azure Logic Apps** and **Azure Service Bus** can be used to decouple a Purchase Order (PO) processing workflow from downstream enterprise applications.

The solution uses a **Publisher → Service Bus Topic → Subscribers** messaging pattern. Purchase Order data is processed by an Azure Logic App and published to an Azure Service Bus Topic. The Topic provides independent subscriptions for downstream systems such as **SAP** and **Salesforce**.

This architecture allows multiple consuming applications to receive and process the same Purchase Order message independently. The downstream systems do not need to communicate directly with each other, which helps reduce point to point dependencies and supports a more maintainable and scalable integration architecture.

---

## Solution Objectives

The solution demonstrates the following Azure integration capabilities:

- Building an **event driven integration workflow** with Azure Logic Apps.
- Publishing business messages asynchronously through **Azure Service Bus**.
- Using a **Service Bus Topic** to distribute the same Purchase Order message to multiple subscribers.
- Supporting independent processing by **SAP** and **Salesforce**.
- Separating message publishing from downstream message consumption.
- Using **Azure Resource Manager (ARM) templates** to represent infrastructure configuration.
- Demonstrating Azure based integration and messaging patterns that can be applied to enterprise application integration scenarios.

---

## Architecture

The high level integration flow is:

```text
                       +------------------+
                       |   Source System  |
                       +--------+---------+
                                |
                                v
                       +------------------+
                       |  Azure Logic App |
                       +--------+---------+
                                |
                                | Publish PO Message
                                v
                 +-------------------------------+
                 |     Azure Service Bus Topic    |
                 +---------------+---------------+
                                 |
                    +------------+------------+
                    |                         |
                    v                         v
          +------------------+       +----------------------+
          | SAP Subscription |       | Salesforce           |
          |                  |       | Subscription         |
          +------------------+       +----------------------+
                    |                         |
                    v                         v
             SAP Processing          Salesforce Processing
```

### Message Flow

1. A Purchase Order message originates from the source/application layer.
2. The **Azure Logic App** receives and processes the message according to the workflow definition.
3. The Logic App publishes the Purchase Order data to an **Azure Service Bus Topic**.
4. The Service Bus Topic distributes the message independently to configured subscriptions.
5. The **SAP subscriber** receives its copy of the message for SAP related processing.
6. The **Salesforce subscriber** receives its copy of the message for Salesforce related processing.
7. Each downstream system can process the message independently without requiring direct communication with the other subscriber.

---

## Integration Pattern

The core design uses a **Publisher → Topic → Subscribers** pattern.

### Publisher

The Azure Logic App acts as the integration workflow responsible for preparing and publishing the Purchase Order message.

### Topic

The Azure Service Bus Topic provides a central messaging layer. Instead of sending the Purchase Order directly to a single downstream application, the message is published once and made available to multiple subscriptions.

### Subscribers

Independent subscriptions allow different applications to consume the same business event.

In this example:

- **SAP Subscription** – consumes Purchase Order messages for SAP processing.
- **Salesforce Subscription** – consumes Purchase Order messages for Salesforce processing.

This approach reduces direct dependencies between applications and allows additional subscribers to be introduced without redesigning the publisher workflow.

---

## Azure Services

### Azure Logic Apps

Azure Logic Apps is used to implement the integration workflow and orchestrate the Purchase Order processing flow.

Typical responsibilities include:

- Receiving or initiating the Purchase Order workflow.
- Processing the incoming message.
- Preparing the message for downstream publishing.
- Sending the message to Azure Service Bus.
- Implementing workflow level error handling and integration logic.

### Azure Service Bus

Azure Service Bus provides the asynchronous messaging infrastructure.

The solution uses a **Service Bus Topic** because a topic supports multiple independent subscriptions for the same published message.

Key benefits demonstrated by this design include:

- Asynchronous communication
- Loose coupling
- Multiple consumers
- Independent message processing
- Scalability
- Reliable enterprise messaging

### Azure Resource Manager (ARM) Templates

ARM templates are included to represent infrastructure configuration and support repeatable deployment of Azure resources.

Infrastructure as Code helps reduce manual configuration and provides a consistent way to recreate environments.

---

## Repository Structure

All three project files are intentionally kept in the **root of the repository**. No separate `RG_APIM` folder is required or used in the GitHub repository.

```text
Azure-LogicApp-ServiceBus-Integration/
│
├── Receive_PO.json
├── template.json
├── parameters.json
└── README.md
```

### File Description

| File | Description |
|---|---|
| `Receive_PO.json` | Azure Logic App workflow definition for the Purchase Order integration flow. |
| `template.json` | ARM template containing Azure resource and deployment configuration. |
| `parameters.json` | Parameter values used with the ARM deployment template. |
| `README.md` | Documentation describing the architecture, workflow, Azure services, and integration pattern. |

> **Security note:** Do not commit real passwords, client secrets, API keys, access keys, connection strings, tokens, certificates, or other sensitive production configuration to a public repository. Replace environment specific secrets with placeholders or example parameters before publishing.

---

## Key Integration Concepts

### 1. Event Driven Architecture

The solution follows an event driven approach in which business information is published as a message rather than requiring synchronous point-to-point communication between applications.

### 2. Asynchronous Messaging

Azure Service Bus enables the publisher and subscribers to communicate asynchronously. The publisher does not have to wait for each downstream application to complete its processing before continuing.

### 3. Loose Coupling

The source/integration workflow is separated from downstream applications through the Service Bus messaging layer.

This means changes to one consumer can generally be handled independently from the publisher and other consumers.

### 4. Publish Subscribe Pattern

A single Purchase Order message is published to the Service Bus Topic and made available to multiple subscriptions.

This is useful when the same business event needs to be consumed by several applications.

### 5. Independent Consumer Processing

SAP and Salesforce can process their respective copies of the Purchase Order message according to their own business and technical requirements.

### 6. Scalability

Additional subscribers can be introduced to the Topic without requiring the publisher to create a separate direct integration path for every new application.

### 7. Maintainability

Using a centralized messaging layer can reduce the number of direct application to application dependencies and simplify long term integration maintenance.

### 8. Infrastructure as Code

ARM templates allow Azure resources and configuration to be represented as deployable code rather than relying only on manual portal configuration.

### 9. Error Handling

The Logic App workflow can incorporate error handling and controlled processing so that integration failures can be identified and managed as part of the workflow.

---

## Business Scenario

A typical enterprise Purchase Order scenario can be represented as:

```text
Purchase Order Created
        |
        v
Azure Logic App
        |
        v
Azure Service Bus Topic
        |
        +--------------------+
        |                    |
        v                    v
   SAP Subscription    Salesforce Subscription
        |                    |
        v                    v
   SAP Processing      Salesforce Processing
```

The same Purchase Order event can therefore be consumed by multiple enterprise applications without creating direct point-to-point integrations between those applications.

---

## Why Azure Service Bus Topic?

A **Topic** is appropriate for this design because the same message needs to be consumed independently by more than one application.

For example:

```text
Purchase Order #1001
        |
        v
Service Bus Topic
        |
        +------> SAP Subscription
        |
        +------> Salesforce Subscription
```

The publisher sends the message once, while each subscription receives its own copy for processing.

This is different from a point-to-point queue pattern where a message is normally intended for a single competing consumer flow.

---

## Deployment Concept

The repository contains Azure deployment artifacts that can be used as part of an Infrastructure-as-Code deployment process.

A high-level deployment approach is:

1. Create or select the target Azure subscription.
2. Create or select the target resource group.
3. Review the ARM template parameters.
4. Provide environment-specific parameter values.
5. Deploy the required Azure resources.
6. Configure the Logic App workflow and Service Bus messaging components.
7. Configure Topic subscriptions for the downstream consumers.
8. Validate end-to-end Purchase Order message processing.
9. Monitor workflow executions and messaging behavior.

The exact deployment values depend on the Azure environment in which the solution is deployed.

---

## Validation and Testing

A basic end-to-end validation approach is:

### Test 1 — Publish a Purchase Order

Trigger the Logic App with a valid Purchase Order message.

### Test 2 — Validate Logic App Execution

Confirm that the Logic App workflow completes the expected processing steps and publishes the message to Service Bus.

### Test 3 — Validate Service Bus Topic

Confirm that the Purchase Order message reaches the Service Bus Topic.

### Test 4 — Validate SAP Subscription

Confirm that the SAP subscription receives the message independently.

### Test 5 — Validate Salesforce Subscription

Confirm that the Salesforce subscription also receives the same Purchase Order message independently.

### Test 6 — Validate Independent Processing

Verify that processing in one subscriber does not require the other subscriber to be available or complete its processing first.

---

## Monitoring and Troubleshooting

For an Azure implementation, monitoring can be performed using the Azure portal and the monitoring capabilities associated with Logic Apps and Service Bus.

Useful areas to review include:

- Logic App run history
- Workflow action inputs and outputs
- Workflow failures
- Service Bus Topic activity
- Subscription message counts
- Dead-lettered messages, where applicable
- Azure resource deployment status
- Integration configuration and connection settings

When troubleshooting, start with the message path:

```text
Source
  ↓
Logic App
  ↓
Service Bus Topic
  ↓
Subscription
  ↓
Consumer Application
```

This makes it easier to determine where a message stopped or where an integration configuration issue occurred.

---

## Technologies Used

- Microsoft Azure
- Azure Logic Apps
- Azure Service Bus
- Azure Service Bus Topics
- Azure Service Bus Subscriptions
- Azure Resource Manager (ARM)
- JSON
- Event-driven integration
- Enterprise application integration

---

## Skills Demonstrated

This project demonstrates practical experience with:

- Azure Integration Services
- Cloud integration architecture
- Messaging and asynchronous integration
- Publish-subscribe architecture
- Azure Logic Apps workflow design
- Azure Service Bus
- Enterprise application integration
- SAP integration patterns
- Salesforce integration patterns
- Infrastructure as Code using ARM templates
- Integration error handling and troubleshooting

---

## Project Highlights

- Implements a **decoupled enterprise integration pattern**.
- Uses **Azure Logic Apps** for workflow orchestration.
- Uses **Azure Service Bus Topic** for asynchronous publish-subscribe messaging.
- Supports independent **SAP** and **Salesforce** subscriptions.
- Demonstrates reusable Azure integration architecture concepts.
- Includes **ARM templates** for Infrastructure-as-Code deployment.
- Provides a practical example suitable for Azure integration and modernization scenarios.

---

## Important Note

Azure resource names, connection details, endpoints, subscription identifiers, credentials, and other environment-specific values should be replaced with the appropriate values for each deployment environment.

Never publish confidential or production credentials in a public GitHub repository.

---

## Author

**Vishal Sharma**

GitHub Profile: https://github.com/vpng2019

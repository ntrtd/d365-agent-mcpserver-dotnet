# Dynamics 365 AI Agent - D365 MCP Server (.NET) (d365-agent-mcpserver-dotnet)

This repository contains the source code for the .NET Core implementation of the Dynamics 365 Model Context Protocol (MCP) Server. This server is a key component of the D365 AI Agent system, responsible for exposing Dynamics 365 business logic and data operations as standardized MCP tools.

## Overview

The `d365-agent-mcpserver-dotnet` acts as a dedicated gateway to Dynamics 365. It receives MCP tool execution requests from clients (primarily the `d365-agent-orchestrator` via the `d365-agent-mcpclient-ts` library) and translates them into operations against the D365 OData API.

## Key Features & Responsibilities

*   **MCP Tool Implementation:** Defines and implements a suite of MCP tools that encapsulate specific Dynamics 365 operations (e.g., `getCustomerDetails`, `createSalesOrder`, `simulateInvoicePostToD365`, `getPurchaseOrderStatus`).
*   **D365 OData Interaction:** Utilizes a generated, type-safe C# OData client (from the `d365-agent-odataclient-dotnet` repository) to perform all communications with the Dynamics 365 OData endpoints.
*   **D365 Authentication:** Manages secure authentication (typically Client Credentials flow) with Dynamics 365, retrieving necessary credentials from Azure Key Vault.
*   **Multi-Instance Support:** Designed to handle requests targeting multiple Dynamics 365 instances or legal entities, based on parameters passed in the MCP tool calls.
*   **Standardized Interface:** Exposes D365 capabilities via the Model Context Protocol, allowing any MCP-compliant client to discover and use its tools.
*   **Error Handling:** Implements robust error handling for D365 operations and communicates errors back via standard MCP error responses.
*   **Containerization:** Packaged as a Docker container for deployment to Azure services like Azure Container Apps (ACA) or Azure Kubernetes Service (AKS).

## Technology Stack

*   **.NET Core / ASP.NET Core** (for hosting the MCP server endpoint)
*   **C#**
*   **D365 Agent C# MCP SDK** (from `submodules/csharp-sdk` or a corresponding NuGet package)
*   **Generated D365 OData Client** (from `d365-agent-odataclient-dotnet`)
*   **MSAL.NET** (for Azure AD authentication)

## Interaction Flow

1.  The `d365-agent-orchestrator` (specifically a LangGraph agent or a CopilotKit action using `d365-agent-mcpclient-ts`) sends an MCP `CallTool` request to this server.
2.  The `d365-agent-mcpserver-dotnet` receives the request.
3.  It identifies the requested MCP tool and validates parameters.
4.  The tool implementation logic uses the generated D365 OData client (`d365-agent-odataclient-dotnet`) to perform the required operations against the Dynamics 365 OData API (e.g., querying data, creating/updating entities).
5.  The result or any error from D365 is packaged into an `McpResponse`.
6.  The `McpResponse` is sent back to the `d365-agent-orchestrator`.

## Getting Started

(Details to be added - typically involves cloning, .NET SDK setup, configuration for D365 environment and Azure Key Vault, and running the server).
Refer to the `implementation.md` in this repository for a detailed development plan.

## Contribution

(Details on contribution guidelines if applicable)

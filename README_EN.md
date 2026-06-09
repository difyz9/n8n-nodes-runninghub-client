
# @difyz9/n8n-nodes-runninghub-client

[![npm version](https://img.shields.io/npm/v/%40difyz9%2Fn8n-nodes-runninghub-client.svg)](https://www.npmjs.com/package/@difyz9/n8n-nodes-runninghub-client)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

English | [简体中文](README.md)

This is an n8n community node that lets you use [Runninghub](https://www.runninghub.cn) in your n8n workflows.

Runninghub is a cloud-based ComfyUI task execution platform that provides powerful AI image/video generation capabilities through an easy-to-use API.

[n8n](https://n8n.io/) is a [fair-code licensed](https://docs.n8n.io/sustainable-use-license/) workflow automation platform.

## Table of Contents

- [Installation](#installation)
- [Operations](#operations)
- [Credentials](#credentials)
- [Compatibility](#compatibility)
- [Usage](#usage)
- [Resources](#resources)
- [Version History](#version-history)

## Installation

Follow the [installation guide](https://docs.n8n.io/integrations/community-nodes/installation/) in the n8n community nodes documentation.

### Community Node Installation

1. Go to **Settings > Community Nodes** in your n8n instance
2. Select **Install**

3. Enter `@difyz9/n8n-nodes-runninghub-client` in the **Enter npm package name** field
4. Agree to the risks and select **Install**

After installation, the **Runninghub Api** node will be available in your workflow editor.

## Operations

This node supports **24 operations** across **6 resources**. The resource groups now match the top-level categories in the official RunningHub API documentation.

### 📋 Account
- **Get Account Info** - Retrieve account status including RH coins, running tasks, wallet balance, and API type
- **Get API Key List** - List API keys under the current account
- **Get Queue Status** - Check queue status for the current API key

### 🚀 ComfyUI Workflow
- **Create ComfyUI Task Simple** - Run a workflow directly without overriding node parameters
- **Create ComfyUI Task** - Create a ComfyUI task based on a workflow template
- **Query Workflow Info** - Get workflow JSON configuration
- **Run Workflow V2** - Call `POST /openapi/v2/run/workflow/{workflowId}` for published workflow APIs that use bearer auth and v2 polling
- **Cancel ComfyUI Task** - Cancel a running or queued ComfyUI task

### 📡 Task Query & Webhook
- **Query Task Status** - Query task status (QUEUED, RUNNING, SUCCESS, FAILED)
- **Query Task Output** - Retrieve task results (images, videos, etc.) from the legacy endpoint
- **Query Task Output V2** - Retrieve task results from the v2 query endpoint
- **Get Webhook Detail** - Inspect webhook event details for a task
- **Retry Webhook** - Resend a specific webhook event

### 🤖 AI App
- **Run AI App** - Initiate an AI application task
- **Get API Call Demo** - Get API call example for AI applications
- **List Public Models** - Query public model resources from the RunningHub model library

### 🧠 Standard Model API
- **Increase Video FPS** - Call `POST /openapi/v2/rhart-video/video-fps-increaser` with a structured `videoUrl`
- **Run Model API** - Call any standard model endpoint under `/openapi/v2/...` by providing the endpoint path and JSON body
- **Preview Model API Price** - Preview pricing for any standard model API request using `/openapi/v2/price-preview/...`
- **Text To Audio Speech 2.8 Turbo** - Call `POST /openapi/v2/rhart-audio/text-to-audio/speech-2.8-turbo` with structured TTS parameters such as `voice_id`, `emotion`, `speed`, `volume`, and `pitch`
- **Upscale Video** - Call `POST /openapi/v2/rhart-video/video-upscaler` with structured `videoUrl` and `targetResolution`

### 📁 Resource Upload
- **Upload File** - Upload images, videos, audio, or ZIP files through `/openapi/v2/media/upload/binary` (max 30MB)
  - Response includes both `download_url` for standard model APIs and `fileName` for ComfyUI workflow nodes

- **Upload Resource (Deprecated)** - Upload files through the legacy deprecated `/task/openapi/upload` endpoint
  - Supported formats: JPG, PNG, JPEG, WEBP (images), MP4, AVI, MOV, MKV (videos), MP3, WAV, FLAC (audio), ZIP (archives)
- **Get Lora Upload URL** - Get upload URL for Lora model files

## Credentials

### Prerequisites

1. Sign up for a Runninghub account at [https://www.runninghub.cn](https://www.runninghub.cn)
2. Generate a RunningHub API key from your RunningHub account settings
3. Create or manage your node license key at [https://licensestore.org/products/n8n-nodes-runninghub-client](https://licensestore.org/products/n8n-nodes-runninghub-client)

### Setting up credentials in n8n

1. In n8n, go to **Credentials > New**
2. Search for **Runninghub Api**
3. Enter your **RunningHub API Key** generated in RunningHub
4. Enter your **License Key** from [License Store](https://licensestore.org/products/n8n-nodes-runninghub-client)
5. Click **Save**

### Testing credentials

The credentials include a built-in test that verifies your API key by calling the account status endpoint.

### License Key and Online Validation

This node requires a valid license key in addition to your RunningHub API key. If you do not have one yet, create it from the official product page: [https://licensestore.org/products/n8n-nodes-runninghub-client](https://licensestore.org/products/n8n-nodes-runninghub-client).

Before executing RunningHub requests, the node performs an online license check through the hosted validation endpoint at `https://open.licensestore.org/v1/license/check`. The license service receives the n8n `instanceId`, `instanceBaseUrl`, selected `resource`, selected `operation`, and a hash of the configured RunningHub API key for license binding and short-lived lease renewal.

The built-in credential test still only validates the RunningHub API key. License validation happens during node execution.

## Compatibility

- **Minimum n8n version**: 1.0.0
- **Tested against**: n8n 1.0.0+
- **Node API Version**: 1

This node has been tested and is compatible with the latest versions of n8n.

## Usage

### Basic Workflow Example

Here's a simple workflow to create a ComfyUI task and retrieve the results:

1. **Add Runninghub Api node** to your workflow
2. **Select Resource**: ComfyUI Workflow
3. **Select Operation**: Create ComfyUI Task
4. **Enter Workflow ID**: Your ComfyUI workflow template ID
5. **Optionally customize node parameters** by enabling "Send NodeInfo List"
6. **Add another Runninghub Api node** to poll for task completion
7. **Select Resource**: Task Query & Webhook
8. **Select Operation**: Query Task Output
9. **Use the Task ID** from the previous node

### Working with Node Parameters

When creating tasks, you can customize node parameters using the `nodeInfoList` field:

```json
[
  {
    "nodeId": "6",
    "fieldName": "text",
    "fieldValue": "A beautiful sunset over mountains"
  },
  {
    "nodeId": "3",
    "fieldName": "seed",
    "fieldValue": "12345"
  }
]
```

### Simple and Advanced ComfyUI Tasks

Use `Create ComfyUI Task Simple` when you want the documented "run as-is" behavior from RunningHub's simple task API. It only sends `workflowId` plus optional `addMetadata` and `accessPassword`.

### Advanced ComfyUI Task Options

The `Create ComfyUI Task` operation also exposes the current advanced workflow options from the RunningHub docs, including `accessPassword`, `addMetadata`, `webhookUrl`, `workflow` JSON override, `instanceType`, `usePersonalQueue`, and `retainSeconds`.

### Published Workflow V2 API

Use the `ComfyUI Workflow` resource when the RunningHub API page shows a path like `/openapi/v2/run/workflow/{workflowId}` instead of the older `/task/openapi/create` style.

This operation:

1. Sends bearer-authenticated requests to `POST /openapi/v2/run/workflow/{workflowId}`
2. Reuses the existing `nodeInfoList` JSON or structured editor
3. Returns a task record that should usually be polled with `Task Query & Webhook -> Query Task Output V2`

### Structured ComfyUI Workflow Editing

ComfyUI workflow tasks now support two ways to provide `nodeInfoList`:

1. `JSON` mode for pasting the full `nodeInfoList` array manually
2. `Structured Items` mode for adding `nodeId`, `fieldName`, and typed `fieldValue` entries one by one

The `Query Workflow Info` operation can also parse the returned workflow prompt and add a `nodeInfoListPreview` array to the output, which makes it easier to identify editable workflow fields before calling `Create ComfyUI Task` or `Run Workflow V2`.

If you only want the prompt template array and do not need the raw workflow JSON, use `Get Workflow Prompt Template` instead. It returns a minimal `nodeInfoListTemplate` payload for the specified workflow ID.

When prompt parsing is enabled, the same response also includes:

1. `workflowPromptFields.system_prompt_fields`
2. `workflowPromptFields.prompt_fields`
3. `workflowPromptFields.negative_prompt_fields`
4. `nodeInfoListTemplate`, which is a direct `nodeInfoList`-style array you can copy into task requests
5. `workflowPromptNodeInfoList` with `promptKind` values of `system`, `prompt`, or `negative`

Example:

```json
{
  "nodeInfoListTemplate": [
    {
      "nodeId": "30",
      "fieldName": "prompt",
      "fieldValue": "主体：延时摄影下，荧光蘑菇群落每 30 秒同步亮起..."
    },
    {
      "nodeId": "19",
      "fieldName": "negative_prompt",
      "fieldValue": "色调艳丽，过曝，静态，细节模糊不清..."
    }
  ],
  "workflowPromptFields": {
    "system_prompt_fields": [
      {
        "node_id": "564",
        "field_name": "text",
        "class_type": "CLIPTextEncode",
        "meta_title": "System Prompt",
        "current_value": "You are a cinematic video director..."
      }
    ],
    "prompt_fields": [
      {
        "node_id": "566",
        "field_name": "text",
        "class_type": "CLIPTextEncode",
        "meta_title": "Positive Prompt",
        "current_value": "A woman turns back and looks at the camera..."
      }
    ],
    "negative_prompt_fields": [
      {
        "node_id": "328",
        "field_name": "text",
        "class_type": "CLIPTextEncode",
        "meta_title": "Negative Prompt",
        "current_value": "worst quality, watermark, subtitles..."
      }
    ]
  },
  "workflowPromptNodeInfoList": [
    {
      "nodeId": "564",
      "fieldName": "text",
      "fieldValue": "You are a cinematic video director...",
      "promptKind": "system"
    }
  ]
}
```

### Generic Standard Model Calls

Use the `Standard Model API` resource when the online documentation exposes a model endpoint that does not have a dedicated n8n operation yet.

1. Select **Resource**: Standard Model API
2. Select **Operation**: Run Model API or Preview Model API Price
3. Enter the endpoint path relative to `/openapi/v2`, such as `kling-video-o3-std/video-edit`
4. Paste the request body JSON from the RunningHub API docs


### Structured Standard Model Operations

Use the `Standard Model API` resource when you want dedicated forms for a few high-frequency standard model APIs instead of manually building JSON bodies.

1. Select **Resource**: Standard Model API
2. Choose **Upscale Video**, **Increase Video FPS**, or **Text To Audio Speech 2.8 Turbo**
3. For video jobs, pass a public video URL or the `download_url` returned by `Upload File`
4. For TTS jobs, fill the structured voice, emotion, and pronunciation settings instead of building raw JSON by hand

### File Upload Example

To upload an image for use in a workflow:

1. **Add Runninghub Api node**
2. **Select Resource**: Resource Upload
3. **Select Operation**: Upload File
4. **Enter file path** or reference from previous node
5. **Use the returned fileName** in your task's nodeInfoList, or `download_url` in standard model API requests

Use `Upload Resource (Deprecated)` only when you specifically need the older `/task/openapi/upload` endpoint.

### Webhook Integration

For long-running tasks, use webhooks to get notified when tasks complete:

1. Set up a webhook endpoint in your n8n workflow
2. When creating a task, add the `webhookUrl` parameter
3. Runninghub will POST results to your webhook when the task finishes

## Resources

- [n8n community nodes documentation](https://docs.n8n.io/integrations/#community-nodes)
- [Runninghub Official Website](https://www.runninghub.cn)
- [Runninghub API Documentation](https://www.runninghub.cn/runninghub-api-doc-cn/)
- [License Key Portal](https://licensestore.org/products/n8n-nodes-runninghub-client)
- [ComfyUI Documentation](https://github.com/comfyanonymous/ComfyUI)
- [Project Structure](docs/PROJECT_STRUCTURE.md)
- [Development Guide](docs/DEVELOPMENT_GUIDE.md)
- [Implementation Summary](docs/IMPLEMENTATION_SUMMARY.md)

## Version History

### 0.2.5 (Current)
- Updated License Key acquisition and online validation documentation, directing users to License Store to create or manage keys
- Added RunningHub community support information with QQ group `484184109`
- Synchronized the README version history with the current npm package version
- Improved n8n credential field guidance to clearly separate the RunningHub API Key from this node's License Key

### 0.2.0 - 0.2.4
- Added v2 account operations for API key list and queue status
- Added AI App public model listing
- Added `Create Task Simple` for the documented simple ComfyUI task entrypoint
- Switched `Upload File` to the current `/openapi/v2/media/upload/binary` API
- Added `Upload Resource (Deprecated)` for the legacy `/task/openapi/upload` API
- Added a standard model helper resource with structured video upscaling, video FPS enhancement, and Speech 2.8 Turbo TTS operations
- Added a generic `Model API` resource for `/openapi/v2/...` endpoints and price previews
- Exposed advanced ComfyUI task parameters such as `retainSeconds`, `instanceType`, and `webhookUrl`
- Added parsed workflow prompt outputs for `Query Workflow Info`, including grouped `system`, `prompt`, and `negative` fields

### 0.1.1
- Expanded the node to 6 resources and 24 operations
- Added AI App, Resource Upload, and Task Query & Webhook resource groups
- Enhanced all operations with detailed descriptions
- Fixed cancelTask export naming
- Improved parameter validation and error handling

### 0.1.0
- Initial release
- Basic Task and Account operations
- API Key authentication

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

[MIT](LICENSE.md)

## Support

If you encounter any issues or have questions:

1. Check the [Runninghub API Documentation](https://www.runninghub.cn/runninghub-api-doc)

2. Open an issue on [GitHub](https://github.com/difyz9/n8n-nodes-runninghub-client/issues)

3. Join the RunningHub QQ group: `484184109`

<img src="icons/20260609181915_7_337.jpg" alt="RunningHub QQ group QR code" width="360" />

4. Contact the Runninghub support team

## Acknowledgments

- Built with [n8n-node-dev](https://github.com/n8n-io/n8n/tree/master/packages/node-dev)
- Integrates with [Runninghub](https://www.runninghub.cn) ComfyUI cloud platform
- Maintained in [difyz9/n8n-nodes-runninghub-client](https://github.com/difyz9/n8n-nodes-runninghub-client)

---

**Made with ❤️ for the n8n community**

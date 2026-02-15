---
name: Modellix
description: Documentation and capabilities reference for Modellix
metadata:
    mintlify-proj: modellix
    version: "1.0"
---

## Capabilities

Modellix is a unified API platform that enables agents to access 100+ cutting-edge AI models for generative tasks. Agents can generate high-quality images and videos from text prompts, transform images using various editing techniques, convert images to videos, and perform advanced video generation with features like multi-shot narratives, audio dubbing, and frame-based control. The platform provides transparent pricing, comprehensive logging, and enterprise-grade reliability with 15 years of IT service expertise.

## Skills

### Image Generation
- **Text-to-Image (T2I)**: Generate images from text prompts using models like Qwen Image Plus, Seedream 4.5, and MiniMax Image-01
- **Image-to-Image (I2I)**: Transform existing images with text instructions using models like Seedream 4.5 I2I, Wanx Image Edit Plus, and MiniMax Image-01 I2I
- **Image Editing**: Perform specialized editing tasks including background generation, sketch-to-image conversion, style repainting, and text-based image editing
- **Artistic Text Generation**: Create stylized text effects with WordArt Semantic (text deformation) and WordArt Texture (material/texture application)

### Video Generation
- **Text-to-Video (T2V)**: Create videos from text descriptions using models like Wan 2.6 T2V, Seedance 1.5 Pro, and Hailuo 2.3 T2V
- **Image-to-Video (I2V)**: Convert static images to dynamic videos with models like Wan 2.6 I2V, Seedance 1.5 Pro I2V, and Hailuo 2.3 I2V
- **First-and-Last-Frame Video (KF2V)**: Generate smooth videos by providing starting and ending frame images with Wan 2.2 KF2V Flash and Wanx 2.1 KF2V Plus
- **Multi-Shot Narratives**: Create complex video sequences with multiple scenes using Wan 2.6 and Seedance 1.5 Pro
- **Audio Integration**: Add automatic dubbing or upload custom audio files to videos (Wan 2.5+, Seedance 1.5 Pro)
- **Professional Camera Control**: Generate cinematic videos with precise camera movements using MiniMax T2V-01-Director and similar models
- **Character Consistency**: Maintain consistent character identity across video frames using MiniMax S2V-01

### API Operations
- **Async Task Submission**: Submit generation requests asynchronously via POST endpoints
- **Task Status Querying**: Check task progress and retrieve results using `GET /api/v1/tasks/{task_id}`
- **Result Retrieval**: Access generated resources (images/videos) with metadata including dimensions, format, and generation timestamps
- **Request Logging**: Full-stack logging of all API calls including input parameters, output, timestamps, response time, and cost consumption

### Authentication & Management
- **API Key Management**: Create, view, and delete API keys from the Modellix console
- **Bearer Token Authentication**: Use `Authorization: Bearer YOUR_API_KEY` header for all requests
- **Rate Limiting**: Global limit of 1000 requests/minute with per-API-key limits of 100 requests/minute
- **Concurrent Task Management**: Control concurrent task execution with team-level limits and client-side semaphore control

## Workflows

### Basic Image Generation Workflow
1. Register and log in to Modellix console
2. Navigate to API Key section and create a new API key
3. Submit a POST request to the text-to-image endpoint with your prompt:
   ```bash
   curl --request POST \
     --url https://api.modellix.ai/api/v1/text-to-image/alibaba/qwen-image-plus/async \
     --header 'Authorization: Bearer <your_api_key>' \
     --header 'Content-Type: application/json' \
     --data '{"prompt": "A cute cat playing in a garden on a sunny day"}'
   ```
4. Receive a `task_id` in the response
5. Query the task result using the task_id:
   ```bash
   curl --request GET \
     --url https://api.modellix.ai/api/v1/tasks/{task_id} \
     --header 'Authorization: Bearer <your_api_key>'
   ```
6. Extract the generated image URL from the `result.resources` array
7. Download and use the image (results are retained for 24 hours)

### Video Generation with Audio Workflow
1. Prepare a text prompt describing the desired video
2. Submit a POST request to a video generation endpoint (e.g., Wan 2.6 T2V or Seedance 1.5 Pro)
3. Include optional audio parameters for automatic dubbing or custom audio file upload
4. Receive task_id and poll for completion
5. Retrieve the generated video with integrated audio from the results
6. Extract video URL and metadata (duration, resolution, format)

### Image-to-Video Transformation Workflow
1. Prepare a source image and text prompt describing the desired motion/transformation
2. Submit POST request to an I2V endpoint with image URL/data and prompt
3. For frame-based control, provide both first and last frame images to KF2V models
4. Poll task status until completion
5. Retrieve the generated video with smooth transitions between frames
6. Download video within the 24-hour retention window

### Error Handling Workflow
1. Check HTTP status code and error response format
2. Parse error message for category (e.g., "Invalid parameters", "Authentication failed", "Rate limit exceeded")
3. For 400 errors: Fix parameters and retry
4. For 401 errors: Verify API key and header format
5. For 429 errors: Check `X-RateLimit-Reset` header and implement exponential backoff
6. For 500/503 errors: Retry with exponential backoff (max 3 retries)
7. Log request timestamp, parameters, error code, and complete message for debugging

## Integration

### MCP (Model Context Protocol) Integration
- Connect Modellix Docs MCP server at `https://docs.modellix.ai/mcp` to AI applications
- Enables AI agents to search Modellix documentation directly within coding environments
- Compatible with Cursor, Claude Desktop, and other MCP clients
- Supports filter parameters for version, language, API reference only, and code examples

### Agent Skills Integration
- Install Modellix Skill using `npx skills add https://github.com/Modellix/modellix-skill`
- Available for multiple agent platforms: Claude Code, Cursor, Codex, Antigravity, Gemini CLI, OpenCode, GitHub Copilot
- Alternatively install from Smithery: `npx @smithery/cli@latest skill add modellix/modellix-skill`
- Agents automatically leverage Modellix capabilities for development tasks

### OpenAI Integration
- Use remote MCP servers with OpenAI models via the `mcp` tool type
- Configure server URL as `https://docs.modellix.ai/mcp`
- Set `require_approval` to "never" for seamless integration
- Models can access Modellix documentation and capabilities during response generation

### Provider Ecosystem
- Integrates models from Alibaba (Qwen, Wanx, Wan), ByteDance (Seedream, Seedance), MiniMax (Hailuo, Image-01), and others
- Unified API abstracts provider differences
- Automatic failover and circuit breaker protection for provider unavailability

## Context

### Model Categories & Providers
- **Alibaba**: Qwen Image series, Wanx/Wan video models, WordArt text effects
- **ByteDance**: Seedream image generation, Seedance video generation, SeedEdit image editing
- **MiniMax**: Hailuo video generation, Image-01 multimodal models, T2V-01 with camera control, S2V-01 for character consistency

### Pricing Model
- **Image generation**: Charged per image (USD/img)
- **Video generation**: Charged per second of video duration (USD/sec)
- Transparent pricing with granular unit costs for different models and parameters
- More competitive rates than official pricing for select core models

### Rate Limiting & Quotas
- Global: 1000 requests/minute
- Per API Key: 100 requests/minute
- Per Model: Subject to provider limitations
- Team-level concurrent task limits (typically 3 concurrent tasks)
- Response headers include `X-RateLimit-Limit`, `X-RateLimit-Remaining`, and `X-RateLimit-Reset`

### Response Format
- Success responses: `code: 0` with HTTP 200
- Error responses: `code` equals HTTP status code (400, 401, 404, 429, 500, 503)
- All responses include `message` field with category and details
- Generated resources in `result.resources` array with URL, type, dimensions, format, and role
- Metadata includes image count, request ID, submission time, and completion time

### Task Management
- All generation requests are asynchronous
- Tasks return immediately with `task_id` for polling
- Results retained for 24 hours after generation
- Task status values: pending, processing, success, failed
- Duration field indicates milliseconds from submission to completion

### Enterprise Features
- AWS Singapore infrastructure with redundant deployment
- Rolling upgrades and strict SLA operational standards
- Full request/response logging for debugging and business analysis
- Financial-grade transaction ledger system with real-time cost tracking
- Trilingual support (Mandarin, Cantonese, English) via email and Discord community
- 15-year enterprise IT service expertise backing the platform

---

> For additional documentation and navigation, see: https://docs.modellix.ai/llms.txt
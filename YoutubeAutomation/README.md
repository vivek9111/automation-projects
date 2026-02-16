YouTube Shorts Automation:

    This repository contains the configuration and documentation for an automated system designed to generate and publish YouTube Shorts. The project leverages n8n to integrate multiple APIs, facilitating a fully autonomous content creation and distribution pipeline.

-----------------------

Workflow Overview:

    The automation follows a structured sequence to ensure consistent content delivery:
        Scheduled Trigger: The process initiates four times daily using a Cron-based scheduler.
        Data Retrieval: The workflow queries a Google Sheet to fetch the next pending topic, including relevant keywords and metadata.
        Asset Sourcing: Utilizing the Pexels API, the system identifies and downloads vertical (9:16) video assets matching the retrieved topic.
        Media Processing: A MediaFX node processes the raw assets by merging clips and overlaying audio to produce a finalized 15-second video file.
        YouTube Upload: The completed video is uploaded via the YouTube Data API v3 with pre-defined hashtags and category settings.
        Notification and Logging: Upon successful upload, the system updates the Google Sheet status and sends a confirmation email via the Gmail API.

-----------------------

Technical Setup:

    To deploy this workflow, ensure the following steps are completed within your n8n environment:
        1. n8n Installation
            The system requires a running instance of n8n. If you are deploying via Docker, use the following command to initialize the container with the correct time zone and memory allocation for video processing:

                    Bash
                    docker run -d --name n8n \
                    -p 5678:5678 \
                    -v n8n_data:/home/node/.n8n \
                    -e GENERIC_TIMEZONE=Asia/Kolkata \
                    -e NODE_OPTIONS="--max-old-space-size=2048" \
                    --restart unless-stopped \
                    n8nio/n8n

        2. Workflow Import
            Download the snappyfast-v1.json file from this repository.
            In your n8n dashboard, create a new workflow and select Import from File.
            Verify all nodes are connected and the Cron expression is configured to your requirements.

        3. API Credentials
            You must configure the following credentials within n8n to enable external service communication:
            Google OAuth2: Required for the YouTube Data API, Gmail API, and Google Sheets API. Ensure the https://www.googleapis.com/auth/youtube.upload and https://www.googleapis.com/auth/gmail.send scopes are authorized in your Google Cloud Console project.
            Pexels API: An API key is required to fetch video content.
            Google Sheets ID: Link the Google Sheets node to your specific content database ID.

        4. Environment Configuration
            Timezone: Ensure the global n8n timezone or the specific node timezone is set to Asia/Kolkata to maintain the 11:00, 16:00, 20:00, and 23:00 schedule.
            Memory Management: For stable video rendering, ensure the host system has at least 2GB of RAM allocated to the Node.js process.

-----------------------

Project Structure:

    YoutubeAutomation/: Contains the n8n workflow JSON files.
    README.md: System documentation and setup instructions.
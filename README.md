# azureaiimageanalysispipelinedemo
A personal demo exploring Image Analysis via Python and PowerShell
# Azure Image Analysis Demo 

This project is a hands-on exploration of Azure's Vision services using Python, PowerShell, and VS Code. It captures captions and tags from uploaded images using the `azure-ai-vision-imageanalysis` SDK (v1.0.0), with error handling, logging, and confidence scoring.

## What It Does

- Connects to Azure Vision resource using secure environment variables
- Sends a local image for analysis
- Extracts caption and tags with confidence levels
- Displays results in console and saves logs to `.txt` and `.json`

## Technologies Used

- Python 3.11
- Azure AI Vision SDK
- PowerShell (script orchestration & cleanup)
- VS Code (development environment)

## Output Example

Caption: a waterfall in a mountain Confidence: 0.62
Tags:
- landscape (0.99)
- outdoor (0.99)
- sky (0.96) ...

## Personal Reflections

This demo helped me understand SDK structures, Azure services behavior, and PowerShell quirks during file execution.

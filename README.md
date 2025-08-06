# Azure Image Analysis – Nature Recognition Demo

This project uses Azure's Vision API to analyze a scenic image featuring waterfalls, pine trees, and a sunburst over snowy mountains. The goal is to test how well the API captures natural beauty through captions and tags.

## Image Description

The input image showcases:
- A waterfall cascading over rocks
- Lush green vegetation and tall pine trees
- A majestic mountain with snow patches
- A dramatic sky with a radiant sunburst

## What the Script Does

- Loads Azure credentials from a `.env` file
- Resizes the image to 300x300 pixels for API compatibility
- Sends the image to Azure Vision API for:
  - Caption generation
  - Tag extraction
- Displays results in the console
- Saves output to `.txt` and `.json` files with timestamps

## Observations

- The API returned a caption with ~60% relevance to the image
- Tags included some accurate elements (e.g. “mountain”, “trees”), but missed finer details like “waterfall” or “sunburst”
- This highlights both the power and limitations of AI in recognizing complex natural scenes

## Folder Structure
├── image_analysis.py         # Main script ├── .env                      # Azure credentials ├── images/                   # Original scenic image ├── resized/                  # Resized image (300x300 PNG) ├── results/                  # Output logs (.txt and .json)

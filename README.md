import os
import json
from datetime import datetime
from PIL import Image
from dotenv import load_dotenv

from azure.ai.vision.imageanalysis import ImageAnalysisClient
from azure.ai.vision.imageanalysis.models import VisualFeatures
from azure.core.credentials import AzureKeyCredential
#load credentials from .env
from dotenv import load_dotenv
import os
load_dotenv(dotenv_path=r"dot_env_path")
AZURE_ENDPOINT = os.getenv("AZURE_ENDPOINT")
AZURE_KEY_PRIMARY = os.getenv("AZURE_KEY_PRIMARY")
print("Endpoint loaded:", os.getenv("AZURE_ENDPOINT"))
print("Key loaded:" , os.getenv("AZURE_KEY_PRIMARY"))

client = ImageAnalysisClient(
    endpoint=AZURE_ENDPOINT,
    credential=AzureKeyCredential(AZURE_KEY_PRIMARY)
)
#image paths
IMAGE_PATH = r"image_path"
RESIZED_PATH = r"resized_path"
RESULT_FOLDER = r"result_path"
#resize image with Pillow
try:
    img = Image.open(IMAGE_PATH)
    img_resized = img.resize((300, 300)) #resize to 300x300 pixels
    img_resized.save(RESIZED_PATH, format="PNG")
except Exception as e:
    print("Error during image resizing:", e)
    exit()
#connect to Azure Vision resource
client = ImageAnalysisClient(
    endpoint=AZURE_ENDPOINT,
    credential=AzureKeyCredential(AZURE_KEY_PRIMARY)
)
#Analyze Image for Caption + Tags
try:
    with open(RESIZED_PATH, "rb") as image_data:
        result = client.analyze(
            image_data=image_data.read(),
            visual_features=["Caption", "Tags"],
            gender_neutral_caption=True
        )
    #display results in console
    if result.caption:
        print("\nCaption:", result.caption.text)
        print(f"Confidence: {result.caption.confidence:.2f}")
    else:
        print("No caption returned.")
    print("\nTags:")
    for tag in result.tags:
        print(f" - {tag} (confidence not available)")
        print("Raw tags object:", result.tags)
        print("Type of first tag:", type(result.tags[0]))
        tags = result.tags.get("values", [])
        print("\nTags:")
        for tag in tags:
            print(f" - {tag['name']} ({tag['confidence']:.2f}")
    #save results to .txt and .json
    timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
    folder = r"folder_path"
    txt_path = os.path.join(folder, f"vision_log_{timestamp}.txt")
    json_path = os.path.join(folder, f"vision_log_{timestamp}.json")

    with open(txt_path, "w", encoding="utf-8") as f_txt:
        f_txt.write(f"Caption: {result.caption.text if result.caption else 'None'}\nTags:\n")
        for tag in result.tags:
            f_txt.write(f" - {tag.name} ({tag.confidence:.2f})")
    with open(json_path, "w", encoding="utf-8") as f_json:
        json.dump({
        "caption": result.captio.text if result.caption else None,
        "tags": [{"name": tag.name, "confidence": tag.confidence} for tag in result.tags]
        }, f_json, indent=4)  
    print(f"\nResults saved to:\n - {txt_path}\n - {json_path}")
except Exception as e:
    print("Error during image analysis:", e)   

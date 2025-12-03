# GeoStructOmni
# 🌍 GeoStructOmni Dataset

[](https://creativecommons.org/licenses/by-nc-sa/4.0/)
[](https://github.com/)
[](https://github.com/)

**Wenzheng Zhang**, **Zhanlong Chen**\*, **Chenxing Sun**, **Liling Kong**

> **A comprehensive multi-modal geospatial benchmark integrating Map, RGB, NIR, SAR, and DEM data.**

**GeoStructOmni** represents a significant leap in geospatial data resources, featuring aligned image-text pairs across five distinct modalities. It is designed to support the next generation of **Generalist Geospatial Models**, enabling tasks such as unified generation, cross-modal retrieval, and semantic change detection.

-----

## 📑 Table of Contents

  - [Overview](https://www.google.com/search?q=%23overview)
  - [Dataset Features](https://www.google.com/search?q=%23dataset-features)
  - [Getting Started](https://www.google.com/search?q=%23getting-started)
  - [Usage](https://www.google.com/search?q=%23usage)
      - [📖 Extract Semantic Captions](https://www.google.com/search?q=%23-extract-semantic-captions)
      - [🖼️ Extract Multi-Modal Images](https://www.google.com/search?q=%23-extract-multi-modal-images)
  - [Citation](https://www.google.com/search?q=%23citation)
  - [Notes & License](https://www.google.com/search?q=%23notes--license)

-----

## 🔭 Overview

**GeoStructOmni** tackles the scarcity of multi-modal data in the remote sensing domain. Unlike traditional datasets confined to RGB, this dataset provides a unified view of the earth's surface through different physical sensors and cartographic representations.

| Modality | Description | Key Features |
| :--- | :--- | :--- |
| 🗺️ **MAP** | Cartographic Map Tiles | Topology, Roads, POIs |
| 📸 **RGB** | Optical Satellite Imagery | Visual Texture, Color |
| 🌵 **NIR** | Near-Infrared Imagery | Vegetation Health, Water Boundaries |
| 📡 **SAR** | Synthetic Aperture Radar | Surface Roughness, Structure |
| ⛰️ **DEM** | Digital Elevation Models | Topography, Elevation, Slope |

-----

## 🚀 Getting Started

### 1\. Install Dependencies

Ensure you have the necessary Python libraries installed for binary parsing and image processing:

```bash
pip install pillow numpy
# 'struct' and 'io' are built-in Python libraries
```

### 2\. Prepare Dataset Files

Place the sample binary files in your working directory. These files contain the synchronized multi-modal data:

  * `geostructomni_captions.bin`: Packaged semantic text descriptions.
  * `geostructomni_images.bin`: Packaged multi-modal image tiles.

-----

## 💻 Usage

We provide efficient Python scripts to unpack and visualize the binary data.

### 📖 Extract Semantic Captions

Use this script to read the aligned textual descriptions for each modality.

```python
import os
import struct

def unpack_captions(bin_file):
    print(f"🚀 Starting extraction for: {bin_file}")
    try:
        with open(bin_file, 'rb') as f_in:
            # Read total file count
            file_count = struct.unpack('I', f_in.read(4))[0]
            print(f"Found {file_count} caption entries.")
            
            for i in range(file_count):
                # 1. Read filename length & filename
                name_len = struct.unpack('H', f_in.read(2))[0]
                file_name = f_in.read(name_len).decode('utf-8')
                
                # 2. Read encoding info
                enc_len = struct.unpack('B', f_in.read(1))[0]
                encoding = f_in.read(enc_len).decode('ascii')
                
                # 3. Read content length & content
                content_len = struct.unpack('Q', f_in.read(8))[0]
                content = f_in.read(content_len).decode(encoding)
                
                # Display result
                print(f"\n--- Entry {i+1}/{file_count} ---")
                print(f"📄 File: {file_name}")
                print(f"📝 Caption: {content.strip()}")
        
        print("\n✅ Unpacking completed successfully.")
    
    except Exception as e:
        print(f"❌ Unpacking failed: {str(e)}")

if __name__ == '__main__':
    # Replace with your actual bin file path
    unpack_captions('geostructomni_captions.bin')
```

### 🖼️ Extract Multi-Modal Images

Use this script to extract and verify the pixel data for Map, RGB, NIR, SAR, and DEM images.

```python
import os
import struct
import io
from PIL import Image

def unpack_images(bin_file, sample_size=10):
    print(f"🚀 Starting image extraction for: {bin_file}")
    try:
        with open(bin_file, 'rb') as f_in:
            file_count = struct.unpack('I', f_in.read(4))[0]
            print(f"Found {file_count} image files.")

            for i in range(file_count):
                # 1. Read filename
                name_len = struct.unpack('H', f_in.read(2))[0]
                file_name = f_in.read(name_len).decode('utf-8')
                
                # 2. Read image binary data
                data_len = struct.unpack('Q', f_in.read(8))[0]
                image_data = f_in.read(data_len)
                
                try:
                    # Process image using PIL
                    image = Image.open(io.BytesIO(image_data))
                    width, height = image.size
                    mode = image.mode
                    # Get a sample of pixels
                    pixels = list(image.getdata())
                    
                    print(f"\n--- Image {i+1}/{file_count}: {file_name} ---")
                    print(f"📐 Dimensions: {width}x{height} | 🎨 Mode: {mode}")
                    print(f"📊 Pixel Sample (First {sample_size}): {pixels[:sample_size]}")
                    
                    # Optional: Save image to verify
                    # image.save(f"extracted_{file_name}")

                except Exception as e:
                    print(f"⚠️ Error parsing {file_name}: {str(e)}")
        
        print("\n✅ Image unpacking completed!")
    
    except Exception as e:
        print(f"❌ Critical Error: {str(e)}")

if __name__ == '__main__':
    # Replace with your actual bin file path
    unpack_images('geostructomni_images.bin', sample_size=10)
```

-----

## 📚 Citation

If you find **GeoStructOmni** useful for your research, please consider citing our paper:

```bibtex
@article{GeoStructOmni2025,
  title={GeoOne: A Unified Foundation Model for Zero-Shot Multi-Task Geospatial Understanding and Generation},
  year={2025}
}
```

-----

## ⚠️ Notes & License

  * **Sample Data:** The data provided here are merely sample subsets. The complete **GeoStructOmni** dataset (containing millions of pairs) will be gradually made public following the acceptance of the associated paper.
  * **Copyright:** The data are protected by copyright. Any unauthorized alteration of the images, pixel values, or semantic text content is strictly prohibited.
  * **Non-Commercial Use:** This dataset is released exclusively for **academic research and educational purposes**. Commercial use without explicit written authorization is prohibited.

-----

*Maintained by the GeoOne Research Team.*

# window_detection
---

# Image Augmentation and Processing Script

This script provides functionality for augmenting and processing images and their associated labels. The primary operations include flipping, resizing, padding, and saving images along with their labels.

## Features

- Horizontal and vertical flipping of images and bounding box labels.
- Random color adjustments (brightness, contrast, sharpness).
- Padding images to a square format while updating bounding box labels.
- Resizing images to a target size.
- Saving processed images and their labels to specified directories.

## Dependencies

Ensure you have the following libraries installed:
- `torch`
- `torchvision`
- `Pillow`
- `numpy`
- `matplotlib`

You can install these dependencies using pip:

```sh
pip install torch torchvision pillow numpy matplotlib
```

## Usage

1. **Setting up directories**: The script sets up directories for storing augmented data.
   
   ```python
   new_data_dir = "./Augmented_data"
   if os.path.exists(new_data_dir):
       shutil.rmtree(new_data_dir)
   os.makedirs(new_data_dir, exist_ok=True)
   os.makedirs(os.path.join(new_data_dir, "train/images"), exist_ok=True)
   os.makedirs(os.path.join(new_data_dir, "train/labels"), exist_ok=True)
   os.makedirs(os.path.join(new_data_dir, "val/images"), exist_ok=True)
   os.makedirs(os.path.join(new_data_dir, "val/labels"), exist_ok=True)
   ```

2. **Image and label processing functions**: The script defines several functions for processing images and labels:
   - `hflip(image, labels)`: Horizontally flips the image and updates the bounding box labels.
   - `vflip(image, labels)`: Vertically flips the image and updates the bounding box labels.
   - `random_color_adjust(image)`: Randomly adjusts the color properties of the image.
   - `pad(image, labels)`: Pads the image to make it square and updates the bounding box labels.
   - `resize(image, target_size)`: Resizes the image to the target size.
   - `transformer(image, labels, params)`: Applies transformations to the image and converts labels to a tensor.

3. **Saving validation data**: The script includes a function to save validation data.
   
   ```python
   def save_val_data(dataset, split):
       for i in range(len(dataset)):
           img, labels, path2img = dataset[i]
           img.save(os.path.join(new_data_dir, f"{split}/images/original_{i}.jpg"))
           np.savetxt(os.path.join(new_data_dir, f"{split}/labels/original_{i}.txt"), labels, fmt="%d %.6f %.6f %.6f %.6f")
   ```

4. **Main processing loop**: The `main()` function iterates through image files in a specified directory, processes each image (e.g., resizing), and saves the processed images to the output directory.

   ```python
   def main():
       image_dir = '/content/yolo_data/train/images/'
       output_dir = '/content/yolo_data/train/processed_images/'

       if not os.path.exists(output_dir):
           os.makedirs(output_dir)

       for filename in os.listdir(image_dir):
           if filename.endswith('.jpg') or filename.endswith('.png'):
               img_path = os.path.join(image_dir, filename)
               img = load_image(img_path)

               if img is None:
                   continue

               try:
                   resized_img = img.resize((256, 256))
               except Exception as e:
                   print(f"Error: Failed to resize image {filename} - {e}")
                   continue

               output_path = os.path.join(output_dir, filename)
               try:
                   resized_img.save(output_path)
                   print(f"Processed and saved {filename} to {output_path}")
               except Exception as e:
                   print(f"Error: Failed to save image {filename} - {e}")
                   continue
   ```


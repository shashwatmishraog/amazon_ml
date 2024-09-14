# Image Text Extraction and Named Entity Recognition (NER) Pipeline

## Overview

This project processes images from URLs, extracts text from the images, and identifies specific entities within that text. The extracted entity information is stored in a pandas DataFrame and saved as a CSV file. The system supports parallel processing for efficient handling of large datasets.

### Key Features
- Downloads images from URLs.
- Extracts text captions from images using a pre-trained image-to-text model.
- Performs Named Entity Recognition (NER) on extracted text.
- Extracts relevant entities like quantities, percentages, and cardinals.
- Uses multithreading to process batches of images concurrently.

---

## Requirements

### Python Libraries:
- `pandas`
- `requests`
- `Pillow`
- `transformers`
- `torch`
- `concurrent.futures`
- `tqdm`

Install the dependencies via `pip`:

```bash
pip install pandas requests Pillow transformers torch tqdm
```

---

## Models Used
- **Image-to-Text**: [microsoft/git-base-textcaps](https://huggingface.co/microsoft/git-base-textcaps)
- **NER**: [dbmdz/bert-large-cased-finetuned-conll03-english](https://huggingface.co/dbmdz/bert-large-cased-finetuned-conll03-english)

---

## How to Use

1. **Prepare the CSV file**:
   The input CSV should have two columns:
   - `image_link`: The URL of the image.
   - `entity_name`: The target entity you want to extract from the text (e.g., "quantity", "percentage").

   Example CSV structure:

   | image_link                    | entity_name |
   | ------------------------------| ----------- |
   | http://example.com/image1.jpg  | percentage  |
   | http://example.com/image2.jpg  | quantity    |

2. **Run the script**:
   Set the path to your CSV file in the code. Update the `file_path` variable to point to the file location.

   Example:
   ```python
   file_path = '/path/to/your/csv_file.csv'
   result_df = main(file_path)
   ```

3. **Output**:
   The results will be saved in `output_results.csv`, with an additional column `extracted_value` representing the extracted entities.

4. **Batch Size and Workers**:
   You can adjust the batch size and number of workers (threads) in the `main` function.

   Example:
   ```python
   result_df = main(file_path, batch_size=16, max_workers=4)
   ```

---

## Code Structure

### Functions

- `load_dataset(file_path)`: Loads the input CSV file into a pandas DataFrame.
- `download_image(url)`: Downloads an image from a given URL.
- `extract_text_and_entity(image, entity_name)`: Extracts text from the image and finds the relevant entity closest to the target attribute.
- `process_image(row)`: Processes a single image, downloads it, extracts text, and identifies entities.
- `process_batch(batch)`: Processes a batch of images concurrently.
- `main(file_path, batch_size=16, max_workers=4)`: The main function that manages the workflow. It processes the images in batches and saves the results to a new CSV file.

---

## Example Usage

```python
file_path = '/path/to/your/csv_file.csv'
result_df = main(file_path, batch_size=16, max_workers=4)

# Save results to CSV
result_df.to_csv('output_results.csv', index=False)

# Optional: Print selected columns for verification
print(result_df[['image_link', 'entity_name', 'extracted_value']])
```

---

## Potential Enhancements
- **Error Handling**: Add more granular error handling for missing or broken images.
- **Entity Types**: Extend support to additional entity types as needed.
- **Performance Optimization**: Utilize GPU if available, or explore other optimization methods.

---

## License

This project is licensed under the MIT License.


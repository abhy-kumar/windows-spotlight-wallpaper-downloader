# Windows Spotlight Wallpaper Downloader

This Google Colab program allows you to download wallpaper images directly from the Windows Spotlight APIs. These are the same APIs that Windows 10 and Windows 11 use to fetch lock screen and sometimes desktop wallpaper images.

## Features

*   Fetches images using either **Windows Spotlight API v3** (used by Windows 10, up to 1080p) or **API v4** (used by Windows 11, up to 4K).
*   User-configurable parameters:
    *   API Version (v3 or v4)
    *   Country Code (e.g., `US`, `GB`, `DE`) - affects image selection.
    *   Locale (e.g., `en-US`, `de-DE`) - affects image descriptions and can influence selection.
    *   Number of images to request (for API v4, 1-4 images).
    *   Display resolution hints (for API v3).
*   Downloads images directly to your local machine via your browser.
*   Displays image previews and metadata (like title, dimensions) within the Colab notebook.
*   Saves images with a descriptive filename including the date of download, API version, and title.

## How it Works

The script makes direct requests to the Windows Spotlight content delivery network:
*   **API v3:** `arc.msn.com/v3/Delivery/Placement`
*   **API v4:** `fd.api.iris.microsoft.com/v4/api/selection`

It parses the JSON response from these APIs to extract image URLs, titles, and other metadata. The images are then downloaded and made available to you. The images fetched are the ones currently being offered by the Spotlight service for the specified parameters at the time of script execution.

## How to Use

1.  **Open in Google Colab:**
    *   Go to [Google Colab](https://colab.research.google.com/).
    *   Click on `File` > `Upload notebook...`.
    *   Upload the `windows-spotlight-wallpaper-downloader.ipynb` file from this repository.
    *   Alternatively, if the notebook is public on GitHub, you can open it directly by navigating to `File` > `Open notebook...` > `GitHub` and pasting the URL to the `.ipynb` file.

2.  **Configure Parameters:**
    *   At the top of the first code cell, you'll find a section with configuration parameters:
        ```python
        # --- Configuration ---
        #@markdown ### API Selection and Parameters:
        API_VERSION = "v4"  #@param ["v4", "v3"] {allow-input: false}
        COUNTRY_CODE = "US"  #@param {type:"string"}
        LOCALE = "en-US"  #@param {type:"string"}

        #@markdown ---
        #@markdown ### API v4 Specific:
        #@markdown (Used if API_VERSION is "v4")
        V4_IMAGE_COUNT = 1 #@param {type:"slider", min:1, max:4, step:1}

        #@markdown ---
        #@markdown ### API v3 Specific:
        #@markdown (Used if API_VERSION is "v3")
        V3_DISPLAY_HORZ_RES = 1920 #@param {type:"integer"}
        V3_DISPLAY_VERT_RES = 1080 #@param {type:"integer"}
        ```
    *   Adjust these parameters as needed using the form fields provided by Colab. See "Configuration Parameters" below for details.

3.  **Run the Script:**
    *   Click the "play" button (▶️) to the left of the code cell, or press `Shift + Enter` while the cell is selected.

4.  **Output & Download:**
    *   The script will output information about the API calls and the images it finds.
    *   A preview of each image will be displayed.
    *   A download link/prompt will appear for each image, allowing you to save it to your computer.

## Configuration Parameters

*   **`API_VERSION`**:
    *   `"v4"`: (Recommended) Uses the Windows 11 API. Generally provides higher resolution images (up to 4K) and can return multiple images.
    *   `"v3"`: Uses the older Windows 10 API. Maximum resolution is typically 1080p.
*   **`COUNTRY_CODE`**: A two-letter country code (e.g., `US`, `CA`, `GB`, `DE`, `JP`). This significantly influences which images are returned.
*   **`LOCALE`**: A locale string (e.g., `en-US`, `fr-CA`, `en-GB`, `de-DE`). This affects the language of metadata (titles, descriptions) and can also influence image selection. Should generally be consistent with the `COUNTRY_CODE`.
*   **`V4_IMAGE_COUNT`** (Only for API v4): The number of images to request from the API. Can be from 1 to 4.
*   **`V3_DISPLAY_HORZ_RES` / `V3_DISPLAY_VERT_RES`** (Only for API v3): Your screen's horizontal and vertical resolution. This *may* influence server-side scaling for API v3 to provide an appropriately sized image. Defaults to 1920x1080.

## Filename Convention

Downloaded images are named using the following pattern:
`spotlight_[api_version]_[date]_[index]_[sanitized_title].[extension]`

*   Example: `spotlight_v4_2023-10-27_1_Pretty_Patagonia.jpg`

## Notes and Limitations

*   **Current Images:** The script fetches images that are currently available from the Spotlight API. It cannot retrieve images for a specific past date.
*   **API Changes:** Microsoft may change these undocumented APIs at any time, which could break the script or alter the response structure. The script attempts to be robust in parsing responses but may require updates if significant changes occur.
*   **"No ad available":** Occasionally, API v4 might return a "No ad available" message. This can sometimes be resolved by trying a different User-Agent (the script uses a common one), changing Country/Locale, or simply trying again later.

## Credits for API Information

The understanding and structure of the Spotlight APIs used in this script were informed by community efforts. In particular:
*   API v3 details were found through projects like KoalaBR's spotlight and Biswa96's WinLight.
*   API v4 analysis was notably conducted by ORelio for the Spotlight-Downloader project.

Please credit the respective projects if you use their findings. This script builds upon such publicly shared knowledge.

---

*Disclaimer: This script interacts with APIs that are not officially documented or supported by Microsoft for public use. Use at your own discretion.*

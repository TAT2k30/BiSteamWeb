# BiSteam Study Source

This project is a web-based resource for studying Level 3 Robots. It allows users to search for and view various robot-related PDF documents. The site is built with HTML, CSS, Bootstrap, and jQuery. 

## Features

- **Search Functionality**: Allows users to search for specific robots and filter the list accordingly.
- **PDF Viewer**: Displays PDF documents in a modal for quick viewing.
- **Cross-Device Compatibility**: Includes adjustments to ensure compatibility with devices like iPads.

## Getting Started

### Prerequisites

- A web browser (Google Chrome, Mozilla Firefox, Safari, etc.)
- Internet connection to load external libraries

### Installation

1. Clone the repository to your local machine:
    ```bash
    git clone https://github.com/TAT2k30/BiSteamWeb.git
    ```

2. Navigate to the project directory:
    ```bash
    cd BiSteam-study-source
    ```

3. Open `index.html` in your web browser.

### Usage

1. Use the search bar in the navigation to filter the list of available robot projects.
2. Click on any of the robot items to view the corresponding PDF document.

### Compatibility Notes

- **iPad Issue**: There was a known issue with PDFs not displaying in the modal on iPads. This has been addressed by providing two solutions:
  1. **Using PDF.js**: Ensures better PDF rendering across devices.
  2. **Open in New Tab**: A simpler solution where PDFs open in a new browser tab.

## Code Overview

### HTML

- The main structure is in `index.html`.
- Includes Bootstrap for styling and jQuery for handling DOM manipulation and events.

### CSS

- Custom styles are in `index.css`.

### JavaScript

- Uses jQuery for handling search functionality and PDF display.
- Optional integration with PDF.js for improved PDF rendering.

#### JavaScript Code for PDF.js

```javascript
$(document).ready(function() {
    $('#searchForm').submit(function(event) {
        event.preventDefault();
        const searchInput = $('#searchInput').val().toLowerCase();
        $('.item').each(function() {
            const itemName = $(this).data('name').toLowerCase();
            if (itemName.includes(searchInput)) {
                $(this).show();
            } else {
                $(this).hide();
            }
        });
    });

    // Function to display PDF using PDF.js
    function displayPDF(url) {
        const pdfViewer = $('#pdfViewer');
        const loadingTask = pdfjsLib.getDocument(url);
        loadingTask.promise.then(function(pdf) {
            pdf.getPage(1).then(function(page) {
                const scale = 1.5;
                const viewport = page.getViewport({scale: scale});

                const canvas = document.createElement('canvas');
                const context = canvas.getContext('2d');
                canvas.height = viewport.height;
                canvas.width = viewport.width;

                const renderContext = {
                    canvasContext: context,
                    viewport: viewport
                };
                page.render(renderContext).promise.then(function() {
                    pdfViewer.html(canvas);
                });
            });
        });
    }

    $('.item').click(function() {
        const pdfUrl = $(this).data('pdf');
        if (pdfUrl) {
            displayPDF(pdfUrl);
            $('#pdfModal').modal('show');
        }
    });
});

# GeoJSON Map & Entity List Demo

## Overview
This test site demonstrates a simple workflow for visualizing geographic data extracted from historical sources. It uses a static GeoJSON file and a basic HTML/JavaScript frontend to display both a map and a searchable/filterable list of entities. The demo is intended as a proof-of-concept for students in digital history, showing how data from sources (such as medieval Latin texts) can be extracted, remediated, and published online.

## Logic & Workflow
1. **Data Extraction**: Historical data (e.g., place names, events) is extracted from a source, possibly using OCR for printed or manuscript texts.
2. **GeoJSON Remediation**: The extracted data is structured as a GeoJSON file, with each entity represented as a geographic feature (point) and associated properties (name, category, etc.).
3. **Frontend Visualization**: The HTML page loads the GeoJSON, displays entities on a map (using Leaflet), and provides a searchable/filterable list. Users can filter by name or category, and download the filtered list as CSV.

## Pros
- **Simplicity**: No backend required; everything runs in the browser with static files.
- **Transparency**: The data structure (GeoJSON) is open and easy to inspect or modify.
- **Interactivity**: Users can search, filter, and download data.
- **Extensibility**: The template can be adapted for more complex data, additional fields, or richer map features.

## Cons
- **Scalability**: Not suitable for large datasets or complex queries; all data is loaded into memory.
- **Security**: No authentication or data protection; anyone can access and modify the files if hosted publicly. If the site is hosted from a Git repository (e.g., GitHub Pages), you can protect the branch (such as `main` or `gh-pages`) using branch protection rules. This prevents direct editing, force pushes, or unauthorized changes. Only users with specific permissions can make changes, and you can require pull requests, reviews, or CI checks before updates are merged. This helps secure your public site’s source code from unwanted edits.
- **Limited Functionality**: This demo currently only shows point features and basic filtering. However, GeoJSON also supports polygons, lines, and other geometry types. You could extend the template to display polygons (such as regions or boundaries) or even overlay a rectified historical map (as a GeoTIFF or image layer) on the Leaflet map. Leaflet natively supports displaying polygons, lines, and raster images, so with some additional code, you can visualize more complex spatial data and historical maps alongside point features. No advanced spatial analysis or editing is included in this demo.
- **Manual Data Preparation**: GeoJSON must be created manually or with custom scripts. OCR and remediation are not automated in this demo, but workflows using Google Cloud Vision or Tesseract can automate OCR for Latin texts. A script could help convert extracted data to GeoJSON, though entity recognition, geocoding, and historical context may require more advanced or manual intervention. Alternatively, a student could use TEI (Text Encoding Initiative) XML to mark entities directly in a text file. TEI is well-suited for encoding named entities, places, and other features in historical texts, and can be transformed into GeoJSON or other formats for visualization.

## Example Use Case
A student extracts place names and coordinates from a medieval Latin chronicle using OCR. The data is cleaned and formatted as GeoJSON, then visualized with this template. The result is a simple, interactive map and list that can be shared online, allowing exploration and download of the data.

## How to Use
- Place your GeoJSON file in the project folder.
- Edit the HTML template as needed (e.g., add categories, change map center).
- Open the HTML file in a browser, or serve it with a static file server.
- Use the search and filter controls to explore the data.
- Download the filtered entity list as CSV for further analysis.

## Notes on Performance and Scalability
- For best results, keep the GeoJSON file under 1,000 features. Most browsers will load and render this amount quickly.
- Loading delays may occur with 5,000+ features, and browser freezing is possible above 20,000 features.
- The demo loads all data into memory and renders all features at once. For larger datasets, consider:
  - Splitting data into smaller files or tiles
  - Using server-side filtering or pagination
  - Leveraging database-backed or map server solutions (e.g., GeoServer, PostGIS)
- Feature complexity (number of properties, geometry type) also affects performance.
- Mobile devices and older computers may experience delays with fewer features than modern desktops.

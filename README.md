# Geographic Data Visualization Demo: GeoJSON & TEI XML Approaches

## Overview
This test site demonstrates two complementary approaches for visualizing and interacting with historical geographic data extracted from historical sources:

1. **GeoJSON Approach** ([demo-geojson.html](demo-geojson.html)): A searchable/filterable list of entities with map visualization
2. **TEI XML Approach** ([demo-xml.html](demo-xml.html)): An interactive travel guide that dynamically parses TEI XML to populate a map, links, and raw markup display

Both approaches show how data from sources can be extracted, remediated, and published online as interactive, web-based experiences.

## Live Demo
View the live demonstrations on GitHub Pages:
- **[GeoJSON Demo](https://bds134.github.io/static-server/demo-geojson.html)** - Search and filter entities with interactive map
- **[TEI XML Travel Guide](https://bds134.github.io/static-server/demo-xml.html)** - Dynamic travel guide with markup parsing

---

## Page 1: GeoJSON Map & Entity List ([demo-geojson.html](demo-geojson.html))

### Logic & Workflow
1. **Data Extraction**: Historical data (e.g., place names, events) is extracted from a source, possibly using OCR for printed or manuscript texts.
2. **GeoJSON Remediation**: The extracted data is structured as a GeoJSON file, with each entity represented as a geographic feature (point) and associated properties (name, category, etc.).
3. **Frontend Visualization**: The HTML page loads the GeoJSON, displays entities on a map (using Leaflet), and provides a searchable/filterable list. Users can filter by name or category, and download the filtered list as CSV.

### Pros
- **Simplicity**: No backend required; everything runs in the browser with static files.
- **Transparency**: The data structure (GeoJSON) is open and easy to inspect or modify.
- **Interactivity**: Users can search, filter, and download data.
- **Extensibility**: The template can be adapted for more complex data, additional fields, or richer map features.

### Cons
- **Scalability**: Not suitable for large datasets or complex queries; all data is loaded into memory.
- **Limited Functionality**: This demo currently only shows point features and basic filtering. However, GeoJSON also supports polygons, lines, and other geometry types. You could extend the template to display polygons (such as regions or boundaries) or even overlay a rectified historical map (as a GeoTIFF or image layer) on the Leaflet map. Leaflet natively supports displaying polygons, lines, and raster images, so with some additional code, you can visualize more complex spatial data and historical maps alongside point features. No advanced spatial analysis or editing is included in this demo.
- **Manual Data Preparation**: GeoJSON must be created manually or with custom scripts. OCR and remediation are not automated in this demo, but workflows using Google Cloud Vision or Tesseract can automate OCR for Latin texts. A script could help convert extracted data to GeoJSON, though entity recognition, geocoding, and historical context may require more advanced or manual intervention.

### Notes on Performance and Scalability
- For best results, keep the GeoJSON file under 1,000 features. Most browsers will load and render this amount quickly.
- Loading delays may occur with 5,000+ features, and browser freezing is possible above 20,000 features.
- The demo loads all data into memory and renders all features at once. For larger datasets, consider:
  - Splitting data into smaller files or tiles
  - Using server-side filtering or pagination
  - Leveraging database-backed or map server solutions (e.g., GeoServer, PostGIS)
- Feature complexity (number of properties, geometry type) also affects performance.
- Mobile devices and older computers may experience delays with fewer features than modern desktops.

### Example Use Case
A student extracts place names and coordinates from a medieval Latin chronicle using OCR. The data is cleaned and formatted as GeoJSON, then visualized with this template. The result is a simple, interactive map and list that can be shared online, allowing exploration and download of the data.

### How to Use the GeoJSON Page
- Place your GeoJSON file in the project folder.
- Edit the HTML template as needed (e.g., add categories, change map center).
- Open [demo-geojson.html](demo-geojson.html) in a browser, or serve it with a static file server.
- Use the search and filter controls to explore the data.
- Download the filtered entity list as CSV for further analysis.

---

## Page 2: TEI XML Travel Guide ([demo-xml.html](demo-xml.html))

### Logic & Workflow
1. **Data Markup**: Historical or descriptive text is marked up using TEI (Text Encoding Initiative) XML, with place names encoded as `<placeName>` elements and tagged with geospatial references.
2. **Dynamic Parsing**: The HTML page fetches the TEI XML file and parses it in the browser using JavaScript.
3. **Multi-use Visualization**: The parsed XML is used for three purposes simultaneously:
   - **Interactive Map**: Place references are extracted and displayed as interactive markers on a Leaflet map
   - **Linked Text**: The text content is rendered with clickable site links that highlight the corresponding map marker when clicked
   - **Raw XML Display**: The original TEI markup is displayed for inspection and transparency

### How It Works
- The page loads the TEI XML file (e.g., `demo-nyc-guide-tei.xml`), which contains text with `<placeName>` elements marked with `ref`, `lat`, and `lon` attributes.
- Example: `<placeName ref="empire_state_building" lat="40.7484" lon="-73.9857">Empire State Building</placeName>`
- JavaScript extracts place names and their geospatial coordinates directly from the XML attributes—no separate hardcoded mapping is needed.
- The **Featured Sites (Marked-up Text)** section is dynamically populated by converting XML `<placeName>` elements into clickable `<span>` elements with appropriate CSS classes.
- Clicking a site link highlights it visually (red) and pans the map to show the corresponding marker with a popup.
- The **Map of Featured Sites** section renders all place references from the XML as interactive map markers using the lat/lon coordinates embedded in the markup.
- The **Raw TEI XML** section displays the original XML file as plain text, preserving its structure for inspection and validation.

### Pros
- **Markup-Driven**: Data is stored in a human-readable, semantic markup language (TEI), making it easy to edit and understand.
- **Multi-Purpose**: A single XML file serves as the source for map data, interactive text, and raw display—no duplication.
- **Transparency**: Both the rendered content and the underlying markup are visible on the same page.
- **Standardized Format**: TEI is a widely-used standard in digital humanities, making the data portable and compatible with other TEI tools and workflows.
- **Flexibility**: Additional metadata can be added to `<placeName>` elements or other TEI tags without restructuring the visualization logic.

### Cons
- **Manual Markup**: TEI encoding must be done manually or with specialized tools. OCR solutions do not automatically produce TEI XML.
- **Limited Scale**: Ideal for smaller, curated texts (e.g., travel guides, chronicles). Large documents with thousands of place names may become unwieldy.

### Self-Contained Data Model
- Unlike traditional approaches, this page **does not require a separate coordinates file or hardcoded JavaScript mappings**.
- All geographic data is embedded directly in the TEI XML as `lat` and `lon` attributes on `<placeName>` elements.
- This makes the XML fully self-contained and data-driven: add new sites by simply adding new `<placeName>` elements with coordinates. No JavaScript modifications needed.
- The page is also fully generic—new TEI files can be used by simply changing the fetch URL, and the same code will work automatically.

### Example Use Case
A student creates a TEI-marked travel guide to New York City, tagging major landmarks as `<placeName>` elements with unique refs. The guide is published using this template, allowing readers to click on landmarks in the text to see them highlighted on the map, and to view the underlying TEI markup for educational purposes.

### How to Use the TEI Page
- Create or edit a TEI XML file (e.g., `demo-nyc-guide-tei.xml`).
- Mark place names with `<placeName>` elements and include `ref`, `lat`, and `lon` attributes:
  ```xml
  <placeName ref="unique_id" lat="40.7580" lon="-73.9851">Site Name</placeName>
  ```
- The page automatically fetches the XML, parses it, extracts coordinates from the attributes, and populates the map, featured sites links, and raw XML display.
- No additional configuration is needed—all data is contained in the XML file itself.

### Handling Repeated Place Mentions in Longer Texts

For longer documents where the same place is mentioned multiple times, you can avoid repeating coordinates by defining places once in a `<places>` section (a sibling to `<text>`) and referencing them in the text:

```xml
<TEI xmlns="http://www.tei-c.org/ns/1.0">
  <teiHeader>
    <fileDesc>
      <titleStmt><title>NYC Travel Guide</title></titleStmt>
      <publicationStmt><p>Demo</p></publicationStmt>
      <sourceDesc><p>Original</p></sourceDesc>
    </fileDesc>
  </teiHeader>
  <places>
    <place xml:id="central_park" lat="40.7829" lon="-73.9654">
      <placeName>Central Park</placeName>
    </place>
    <place xml:id="empire_state" lat="40.7484" lon="-73.9857">
      <placeName>Empire State Building</placeName>
    </place>
  </places>
  <text>
    <body>
      <p>
        I visited <placeName ref="#central_park">Central Park</placeName> in the morning. 
        Later, I returned to <placeName ref="#central_park">Central Park</placeName> at sunset.
      </p>
    </body>
  </text>
</TEI>
```

With this standard TEI structure:
- The `<places>` section is a sibling to `<text>` (both children of `<TEI>`)
- Each place is defined once with coordinates
- Multiple mentions in the text use simple `ref="#place_id"` references
- The code can look up coordinates from the `<places>` definitions
- This is the TEI standard for handling repeated entities and following best practices

---

## Security

When hosting either page publicly (e.g., GitHub Pages), you can protect your data source files using branch protection rules:
- If the site is hosted from a Git repository (e.g., GitHub Pages), you can protect the branch (such as `main` or `gh-pages`) using branch protection rules. This prevents direct editing, force pushes, or unauthorized changes.
- Only users with specific permissions can make changes, and you can require pull requests, reviews, or CI checks before updates are merged.
- This helps secure your public site's source code from unwanted edits.

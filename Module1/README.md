# **Module One: Creating Maps**

This module introduces the fundamental workflow of creating maps using QGIS. You will learn to import data layers, apply symbology, add labels, and design layouts for maps. By the end of this module, you will have transformed a text file containing historical earthquake records into an informative visualization.

---

## **1.1 Importing Vector Data**

### **Steps to Import Data**

1. Open **QGIS** and click **Open Data Source Manager**.  
2. Select the **Vector** tab, then click **…** to browse and select `ne_10m_land.shp`. Click **Open** → **Add**.  
   - This layer contains polygons representing the world's land areas.  
3. Repeat the steps above to add `gem_active_faults_harmonized.gpkg`.  
   - This layer represents global active fault lines.  
4. Open **Data Source Manager** again, select **Delimited Text**, and browse for `significant_earthquakes_2000_2020.tsv`.  
5. Configure the **File Format**:
   - Select **Custom delimiters** → Check **Tab**.
   - Set **Longitude** as **X Field** and **Latitude** as **Y Field**.
   - Choose **EPSG:4326** as **Geometry CRS**.
6. Click **Add**. The layer now appears in the **Layers panel**.  

### **Querying and Selecting Records**

1. Open the **Attribute Table** of `significant_earthquakes_2000_2020`.  
2. Click **Select Features by Value**, enter `2020` for **Year**, and click **Select Features**.  
3. Refine the query:
   - Set **Mag** to `Greater than 7` → Click **Select Features**.  
4. Click **Move selection to top** to examine selected features.  
5. Save the top 10 earthquakes by fatalities:
   - Right-click the layer → **Export → Save Selected Features As...**  
   - Choose **GeoPackage**, name it `large_earthquakes.gpkg`, and click **OK**.  

📌 **Checkpoint:** Your output should match `Earthquakes_Checkpoint1.qgz`.  

### **Challenge: Locate Null Island**  
- Open `ne_10m_land` **Attribute Table** and find "Null Island."  
- Select and zoom to it on the map.  

---

## **1.2 Symbology: Styling Layers**

1. Open the **Layer Styling Panel** for `ne_10m_land`.  
   - Set **Fill color**: Light grey.  
   - Set **Stroke color**: White.  
2. Open `gem_active_faults_harmonized`:  
   - Set **Color**: Brown.  
   - Set **Stroke width**: `0.1`.  
3. Open `significant_earthquakes_2000_2020`:  
   - Set **Size**: `0.7 mm`.  
   - Set **Fill color**: Red.  
   - Set **Stroke color**: White, **Width**: `0.1`.  
4. Open `large_earthquakes`:  
   - Use **Proportional Circle Style**.  
   - Set **Total Deaths** as input.  
   - Define **Size range**: `5000 - 500000` deaths → `3 - 10` mm.  
   - Adjust **transparency**.  
   - Add **Data-defined Size Legend** with size classes: `5000, 50000, 500000`.  

📌 **Checkpoint:** Your output should match `Earthquakes_Checkpoint2.qgz`.  

### **Challenge: Add Live Layer Effects**  
- Enable **Draw Effects** in the `large_earthquakes` styling panel.  
- Add a **Drop Shadow** effect to highlight earthquakes.  

---

## **1.3 Labeling Earthquake Locations**

1. **Change the Project CRS** to **WGS84 / Equal Earth Greenwich (EPSG:8857)**.  
2. Open the **Layer Styling Panel** for `large_earthquakes`.  
   - Select **Single Labels**.  
   - Click **Expression** and enter:  

     ```qgis
     "Location Name" ||  ';' || 'Deaths:' || "Total Deaths"
     ```
   - Adjust label formatting:
     - **Wrap on character**: `;`
     - **Wrap lines**: `20 characters`
     - **Size**: `8`, **Color**: `White`
     - **Background**: `Black`, **Buffer**: `1pt`
   - Enable **Callouts** to attach labels to symbols.  
3. Activate the **Label Toolbar** and manually adjust label positions.  

📌 **Checkpoint:** Your output should match `Earthquakes_Checkpoint3.qgz`.  

### **Challenge: Format Numbers with Thousands Separator**  
- Modify the label expression using `format_number()` to display numbers with commas.  

---

## **1.4 Print Layout**

1. **Create a New Print Layout**:  
   - Set **Page Size**: `A4` → **Orientation**: `Landscape`.  
2. **Add Map**:  
   - Adjust **Scale**: `120000000`.  
   - Use **Interactively Edit Map Extent** to refine placement.  
3. **Add Title**:  
   - Text: `10 Largest Earthquakes (2000-2020)`.  
   - Set **Font Size**: `24`, **Alignment**: `Center`.  
4. **Customize the Legend**:  
   - Remove unnecessary layers.  
   - Rename `gem_active_faults_harmonized` to **Faults**.  
   - Rename `large_earthquakes` to **Deaths**.  
5. **Add Images and Credits**:  
   - Insert `Made with QGIS` logo.  
   - Add a label for **Data Sources**.  
6. **Export Map**:  
   - Save as `large_earthquakes.png`.  

📌 **Checkpoint:** Your output should match `Earthquakes_Checkpoint4.qgz`.  

### **Challenge: Export as PDF**  
- **Hint**: Uncheck **Simplify geometries** to maintain accuracy.  

---

## **Summary of Challenges**  

1. **Find Null Island** using the `ne_10m_land` layer.  
2. **Apply a Drop Shadow effect** to `large_earthquakes`.  
3. **Format numbers in labels** using `format_number()`.  
4. **Export the print layout as a PDF**.  

🎯 **By completing this module, you will have learned the core functions of QGIS, including data import, querying, symbology, labeling, and print layout design.**

### **Final Output**
![Final Map Output](large_earthquakes.png)

---

## **Raw Notes**
In Module 1, we are making a global map. The default CRS **EPSG:4326** uses latitude/longitude coordinates, which are **not ideal for visualization**. While it is good for projection and data storage, it **distorts the sizes of continents and affects distance accuracy** when displayed. 

For global maps, it is best to use an **equal-area projection**, as it preserves relative areas of continents. A modern projection designed for this purpose is the **Equal Earth Projection**, which is adapted from the **Robinson Projection** (previously used by National Geographic). **Anytime you want to create a global map, visit [Equal Earth](https://equal-earth.com/).**
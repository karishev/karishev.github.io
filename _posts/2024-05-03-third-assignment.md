---
title: "Assignment 3"
date: 2024-05-03T00:34:30-04:00
categories:
  - assignment
tags:
  - learning
  - spatial-data
  - kazakhstan
  - defense
  - data-visualization
  - kepler.gl
  - my-maps
toc: true
toc_label: "Assignment 3"
toc_sticky: true
toc_icon: "book"
---


### Step 1: Choosing the source
---
For my spatial data assignment, I decided to work with the "Kazakhstan Defense Enterprise Directory, 1995" as my primary source, mainly because I am Kazakh and have never seen such material. This directory is intriguing due to its comprehensive coverage of over 37 defense enterprises transitioning to civilian markets in post-Soviet Kazakhstan. Presented in English, the directory provides detailed profiles, addresses, and business focuses, making it an excellent dataset for mapping and analyzing the geographic and economic landscapes during a pivotal time in Kazakhstan's history.


### Step 2: Modeling The Data
---
I initially attempted to automate the extraction of key information from the directory using ChatGPT since all the main points were similar. I asked ChatGPT to generate a structured dataset directly from the entire directory text. The structure for the dataset was defined as follows:

- **Name of Enterprise**
- **Address**
- **Primary Business**
- **Conversion Projects** (if applicable)
- **Contacts** (Phone numbers and fax)
- **Key Technologies**

However, when I tried to process the full directory through ChatGPT in one go, the AI struggled to accurately parse and organize the data into the specified format for all entries. It kept returning around 5-10 entries, which was not enough. I figured that if it could do 7 in a batch, then I could adapt my approach by manually segmenting the directory into four smaller batches. This cutting made the data more manageable and improved ChatGPT's performance in extracting and structuring the information.

![image](/assets/images/dh-assignment-2.png)

For each batch, I used the following prompt:

_"Extract these details for each enterprise in the attached text file: Name of Enterprise, Address, Primary Business, Conversion Projects, Contacts, and Key Technologies. Make it a table. Thanks"_

This approach worked. Maybe because I thanked it, or it just works wonders itself, but ChatGPT was able to process each batch separately and produce a clearer, more accurate dataset. After extracting the data for all batches, the final dataset had 37 entries. I then manually transferred all extracted data into a Google Sheets document, making sure each entry was correctly formatted and aligned with the specified columns. 

Although the final number of entries was less than 50, the comprehensive coverage of all available data from the directory ensures the dataset is robust and complete for the intended analysis and mapping activities.

### Step 3: Enriching the data
---
Using the extension [Geocode by Awesome Table](https://workspace.google.com/marketplace/app/geocode_by_awesome_table/904124517349) I got the latitude and the longitude for each enterprise. I was surprised since I had never before used an extension for Google Sheets and how genuinely helpful and automated it was. Luckily, each address provided proved to exist and upon checking the map created by the same extension, it looked like everything corresponded correctly.

![image](/assets/images/dh-assignment-2-1.png)

I proceeded to enhance the dataset by adding a classification column named 'Focus'. This classification helps distinguish enterprises based on their primary business activities—either as 'Civilian', 'Military', 'Hybrid', or 'Unclassified'. The classification process involved analyzing the descriptions within the 'Primary Business' and 'Conversion Projects' columns. Given the diverse nature of the enterprises listed in the directory, a Python script was utilized to automate the classification by scanning for specific keywords: 

```python
import pandas as pd

def classify_focus(primary_business, conversion_projects):
	primary_business = primary_business.lower()
	conversion_projects = str(conversion_projects).lower()
	
	military_keywords = ['defense', 'military', 'torpedoes', 'missile', 'naval', 'arms', 'rocket', 'ammunition', 'aircraft', 'submarine', 'tank', 'fighter', 'radar', 'combat']
	
	civilian_keywords = ['medical', 'consumer', 'biotechnology', 'ceramics', 'electronics', 'mining', 'cars', 'healthcare', 'vaccines', 'software', 'energy', 'agricultural', 'tin', 'water', 'tractors', 'plastics', 'construction', 'telecommunications']
	
	  
	is_military = any(keyword in primary_business for keyword in military_keywords)

	is_civilian = any(keyword in primary_business for keyword in civilian_keywords) or any(keyword in conversion_projects for keyword in civilian_keywords)
		
	if is_military and is_civilian:
		return 'Hybrid'
	elif is_military:
		return 'Military
	elif is_civilian:
		return 'Civilian'
	else:
		return 'Unclassified'
		

# Load the CSV file
df = pd.read_csv('./enterprises.csv')

# adding the new row
df['Focus'] = df.apply(lambda row: classify_focus(row['Primary Business'], row['Conversion Projects']), axis=1)

# save the results to a new CSV file
df.to_csv('classified_enterprises.csv', index=False)
```

The resulting table got a new column and looks like this now. I also decided to classify some of the “Unclassified” enterprises so that we can have more data points.

![image](/assets/images/dh-assignment-2-2.png)

By integrating this new 'Focus' column, the dataset not only provides a detailed geographic representation of the enterprises but also offers insight into the nature of their business operations. This addition is crucial for subsequent analyses, such as identifying regional clusters of military versus civilian enterprises.

### Step 4: Visualizing the data
---
For the visualization of the spatial data concerning Kazakhstan's enterprises, I utilized two powerful tools: Kepler.gl and Google My Maps. Each tool brought its unique strengths to the analysis, providing different insights and usability features that were crucial for interpreting the data effectively.

#### Visualization Tools Comparison:

**Kepler.gl:**

![image](/assets/images/dh-assignment-2-7.png)

- **Advanced Features**: Kepler.gl proved invaluable for its advanced visualization capabilities. It allowed for complex filtering based on the 'Focus' classification of each enterprise, enabling me to isolate and analyze groups like 'Military', 'Hybrid', and 'Civilian' with ease.
- **Dynamic Interaction**: The tool's layering and zoom features helped in examining the spatial relationships and patterns with greater detail. For instance, the clustering of military enterprises near the Russian border was visually distinct and could be analyzed in the context of historical military ties between Kazakhstan and Russia.
- **Customization Options**: I could customize color schemes and the legend to reflect different classifications clearly, enhancing the interpretability of the map.

![image](/assets/images/dh-assignment-2-6.png)

**Google My Maps:**

![image](/assets/images/dh-assignment-2-4.png)

- **User-Friendly Interface**: Google My Maps offered a straightforward and intuitive interface, which was excellent for basic mapping needs. It allowed for a quick setup.
- **Limited Filtering Capabilities**: While effective for general mapping, it lacked the filtering capabilities of Kepler.gl. Selecting multiple markers without an intuitive filter option was less efficient, though feasible for smaller datasets or less complex analyses (you can choose the points by pressing CMD + click which is not really intuitive).
- **Simplicity and Accessibility**: Best suited for simpler visual presentations, Google My Maps facilitated quick geographical assessments and was accessible to those who might not be familiar with more advanced GIS tools.

![image](/assets/images/dh-assignment-2-5.png)

#### Insights from Visualization:

- **Strategic Placement of Military Enterprises**: The analysis highlighted a significant concentration of military-related enterprises along the northern border with Russia, emphasizing the strategic and historical military alignment.
- **Distribution of Civilian Enterprises**: Civilian enterprises were dispersed widely, underscoring Kazakhstan's push towards economic diversification across various regions. This distribution also aligns with areas of higher population density, suggesting a market-driven placement.
- **Use of Hybrid Enterprises**: The presence of hybrid enterprises revealed ongoing transitions within the industrial sectors, reflecting adaptability and a move towards integrating civilian applications with existing military capabilities.
- **Empty Middle**: While I knew that the central area of Kazakhstan was low in density, it was interesting to see how there are 0 enterprises located. This suggests higher population density in the border areas.

These visualizations not only confirmed some of the expected geographic and strategic placements but also provided a platform for deeper analysis of how past affiliations and current economic policies shape the industrial landscape of Kazakhstan. 


### Step 5: Conclusion
---
This spatial data project offered valuable insights into Kazakhstan's industrial landscape, with a specific focus on enterprises categorized by military, civilian, and hybrid operations. By employing tools like Kepler.gl and Google My Maps, I effectively visualized the geographical distribution and strategic locations of these enterprises.

#### Reflections:

- **Insights**: The concentration of military enterprises near the Russian border showcases historical ties, while civilian enterprises are broadly distributed, indicating economic diversification efforts.
- **Data Interpretation**: The use of the 'Focus' classification helped in proper data visualization.

#### Future Directions:

Going forward, incorporating additional data layers—such as economic performance or demographic trends—could further enrich the analysis. Exploring the impact of regional policies on these enterprises could also offer deeper insights into Kazakhstan's economic strategies.

This assignment has been instrumental in enhancing my understanding of spatial data's role in economic and strategic analyses and has equipped me with valuable skills in data visualization and interpretation.
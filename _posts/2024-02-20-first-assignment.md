---
title: "Assignment 1"
date: 2024-02-20T00:34:30-04:00
categories:
  - assignment
tags:
  - learning
  - HAM
  - API
  - python
  - data-visualization
---

### Part 1

---

For this part of the assignment, I looked into the myriad of artworks on the Harvard Art Museum (HAM) website, comparing them with the detailed metadata available in the CSV file. This comparative analysis aimed to uncover the depth of information and context provided by each platform, revealing the intrinsic value and limitations inherent in digital and data-driven art curation.

#### HAM website

The website layout is clean and intuitive, judging from user experience, it checks all the marks: accessible, easy to navigate, easy to understand. The website is user-friendly, offering easy navigation and access to a wide range of collections. Information about each art piece is readily available, including the artist, medium, and a brief description. The website's design and functionality enhance the user experience, making it easy to explore and learn about different art pieces. For me, the homepage is comfortable to use, no comments there.

The vision changes slightly when navigating to a specific artwork. As an example, I decided to look into [this artwork](https://harvardartmuseums.org/collections/object/216544?position=216544) that looked interesting to me. The dedicated page provides enriching information about the art piece, talking about every part of it, and being open about each part. However, the website's design, focusing on individual pieces, inadvertently narrows the viewer's perspective, limiting their ability to draw connections between artworks from similar epochs, regions, or thematic groups. I would say, that having a section on the right or after the presentation regarding other pieces with similar history, region, or collection would help draw connections. In this case, it only provides a little text at the end:

==_This page is a presentation of the object's record in our [API](https://github.com/harvardartmuseums). API records are regularly refreshed with data from our catalog. This record was refreshed on February 23, 2024, at 07:44 am._==

#### Comparison to the CSV

Given my last comments regarding the individual presentations of the art pieces, it should be said that the CSV file looks dull and incomprehensible at first. It just looks like a lot of data and _nobody_ wants to look into a lot of data without data representation and data analysis. However, the division into categories and the column names allow for more immersive information about the art pieces. It tackles the problem faced before: it allows us to draw connections. I found the CSV file to be more organized and helpful while not visually pleasing.

### Part 2

---

Glimpsing over the cultures dataset, it's evident that the majority of art pieces are related to European, North American, or East Asian origins. This observation aligns with the historical prominence of these regions over the last two millennia. Among the myriad of cultures represented, the "Ottoman" category caught my attention, not only due to its rich historical tapestry and geographical closeness to Kazakhstan but also the intriguing exact count of 666 objects. Here is a table of the most popular categories with more than 500 objects.

| Country              | Count |
| -------------------- | ----- |
| American             | 90115 |
| German               | 35600 |
| French               | 26040 |
| Italian              | 12501 |
| British              | 11114 |
| Japanese             | 10324 |
| Byzantine            | 9844  |
| Greek                | 9204  |
| Chinese              | 6707  |
| Roman                | 5581  |
| Dutch                | 5094  |
| Persian              | 3121  |
| Roman Provincial     | 2328  |
| European             | 2092  |
| Flemish              | 1797  |
| Korean               | 1604  |
| Indian               | 1485  |
| Croatian             | 1381  |
| Spanish              | 1349  |
| Egyptian             | 1095  |
| Netherlandish        | 951   |
| Roman Imperial       | 778   |
| Unidentified culture | 669   |
| Ottoman              | 666   |
| Swiss                | 627   |
| Roman Republican     | 541   |

To delve deeper, I employed a script in part 1 to pinpoint the most and least popular Ottoman objects based on viewer engagement.

```python
# Sorting by views - most and least popular
df_sorted = df.sort_values(by='totalpageviews', ascending=False)

# Identifying the most and least popular objects
most_popular = df_sorted.iloc[0]
least_popular = df_sorted.iloc[-1]

# Print summary
print("Most Popular Ottoman Object:")
print(f"Title: {most_popular['title']}, Views: {most_popular['totalpageviews']}, URL: {most_popular['url']}\n")

print("Least Popular Ottoman Object:")
print(f"Title: {least_popular['title']}, Views: {least_popular['totalpageviews']}, URL: {least_popular['url']}")`

```

Returned:

```markdown
Most Popular Ottoman Object:
Title: Calligraphic Portrait of the Prophet Muhammad (recto); Geneaology of the Prophet Muhammad (verso), left-hand side of a bifolio from a manuscript, Views: 1244, URL: [https://www.harvardartmuseums.org/collections/object/146656](https://www.harvardartmuseums.org/collections/object/146656)

Least Popular Ottoman Object:
Title: Painting, verso; Text, recto of folio 34, illustrated folio from a manuscript of the Kitab-i Mihasikü’l-Hacc by Mevlana Jami, Views: 0, URL: [https://www.harvardartmuseums.org/collections/object/98360](https://www.harvardartmuseums.org/collections/object/98360)
```

The script revealed:

- **Most Popular Ottoman Object**: A calligraphic portrait of the Prophet Muhammad, viewed 1244 times, suggesting a significant interest likely due to its religious context and detailed depiction. However, the view count seems modest for such a pivotal figure, underscoring perhaps a limited digital reach or specific audience engagement.

- **Least Popular Ottoman Object**: An illustrated folio from the manuscript of the Kitab-i Mihasikü’l-Hacc by Mevlana Jami, with zero views. This piece remains enigmatic, suggesting that its specific religious and cultural significance may not be widely recognized or accessible.

Upon examining the website URLs for these works, the disparity in popularity becomes apparent:

1. **Visual Accessibility**: The most viewed object features a detailed image and description, enriching the visitor's experience and understanding. Conversely, the absence of imagery for the least viewed item significantly hinders engagement.

2. **Content Description**: A comprehensive description and religious context provided for the most popular item contrasts sharply with the scant information available for the least viewed, highlighting the importance of narrative in captivating interest.

3. **Related Works Section**: Interestingly, this feature, which could potentially enhance visitor exploration, appears inconsistently across the two objects. Its presence on the least viewed item's page, yet with missing images and descriptions, presents a missed opportunity for engagement.

Visualization on the website allows for exploration. Also, if there is no visualization, what is the point of the website if I can have a similar image in my head just by reading this in the CSV file?

However, one new feature I learned from the website, is the presence of related works section. I discussed how one would be beneficial at the beginning of the post. I am not sure if having different structures for the data is a good design choice.

![image](/assets/images/relatedworks.png){: width="300px"}

After going to the related art piece of the manuscript I found something extremely funny. This art piece, having the same name and no description and image, has more related works! And most of them have images that allow you to delve into the manuscript. It is interesting because they are from the same book according to the name of it. Therefore, the question arises, why are these "related works" not in the least popular works? Bug of the system? I believe that having those related works with images would allow for better engagement.

![image](/assets/images/relatedworks2.png)

### Part 3

---

Upon analyzing the data from the Harvard Art Museum, I have discovered intriguing patterns that speak volumes about the museum's collection practices and cultural emphasis.

#### Word Cloud Observations:

1. **Ottoman**: The prominence of manuscripts and religious texts in the word cloud reflects the rich literary and religious heritage of the Ottoman Empire. It also suggests a curatorial focus on items that embody the intellectual and spiritual life of the period. It becomes prominent when adding the word "nan" to the stopwords.
   ![image](/assets/images/ottomanwords.png){: width="300px"}
   ![image](/assets/images/ottomanwords_nan.png){: width="300px"}

2. **Islamic**: The frequent occurrence of words like 'Sura' and 'Qu'ran' underscores the significance of Islamic religious artifacts within the collection. This resonates with the historical importance of Islam as a unifying cultural and artistic influence across diverse geographies. The word cloud also has a lot of similarities with the Ottoman one - manuscript, folio, etc.
   ![image](/assets/images/islamicwords.png)

3. **Turkish**: The word cloud for Turkish artifacts reveals a strong emphasis on textiles and ceramics, indicating the museum's appreciation for the material culture and artisanal craftsmanship of the region. Same as in every one of the three cultures, deleting "nan" gives more insight into the word clouds.

![image](/assets/images/turkishwords.png){: width="300px"}
![image](/assets/images/turtkishwords_nan.png){: width="300px"}

After that, I decided to write a code that find words that are common across all three cultures to get a more in-depth view. The code looks like this:

```python
from collections import Counter

def get_words(text):
    # Remove stopwords
    stopwords = set(["this", "for", "nan", "and", "of", "a", "the", "in", "on", "with", "as", "at", "to", "are", "then", "from", "an", "that", "is", "by", "his", "also", "it", "or", "and", "been","has"])
    # Convert to lowercase and split into words
    words = text.lower().split()
    # Remove stopwords
    return [word for word in words if word not in stopwords]

first_culture_words = get_words(first_culture_text)
second_culture_words = get_words(second_culture_text)
third_culture_words = get_words(third_culture_text)

common_words = set(first_culture_words) & set(second_culture_words) & set(third_culture_words)

common_word_counts = Counter(
    first_culture_words + second_culture_words + third_culture_words
)

common_word_counts = {word: count for word, count in common_word_counts.items() if word in common_words}

# Sort the word counts from most to least common
sorted_common_word_counts = sorted(common_word_counts.items(), key=lambda item: item[1], reverse=True)

print("| WORD | COUNT |")
print("| --- | --- |")

for item in sorted_common_word_counts:
    print("| ", item[0], " | ", item[1], " | ")
```

I decided to return it as a table for Markdown for ease of use and to be able to just put it into Markdown and it will automatically make it a table!

| WORD        | COUNT |
| ----------- | ----- |
| two         | 61    |
| floral      | 50    |
| fragment    | 38    |
| pattern     | 37    |
| border      | 36    |
| one         | 36    |
| blue        | 34    |
| coin        | 33    |
| small       | 32    |
| decoration  | 31    |
| lines       | 30    |
| both        | 30    |
| red         | 29    |
| be          | 29    |
| white       | 28    |
| right       | 24    |
| other       | 22    |
| around      | 21    |
| inscription | 21    |
| motifs      | 20    |
| its         | 19    |
| 2           | 19    |
| top         | 17    |

I asked ChatGPT what could these words indicate and here is its answer.

- ==**Numerical Terms ("two," "one")**: These could refer to the number of items in a pair or series, or possibly attributes of the objects (e.g., "two handles").==

- ==**Design Descriptions ("floral," "pattern," "border", "motifs")**: A focus on the intricate patterns and decorative motifs, often found in textiles, ceramics, and illuminated manuscripts, highlighting the complexity and artistry in the designs from these cultures.==

- ==**Physical Attributes ("blue," "red," "small", "white")**: Color descriptors and size references suggest a rich visual diversity and range of scales in the objects collected.==

- ==**Material and Composition ("fragment", "decoration", "lines", "inscription")**: These terms could imply the presence of incomplete artifacts ("fragment") and detailed work ("inscription"), perhaps relics or excavated pieces, as well as items with specific decorative features.==

- ==**Spatial References ("top," "right", "around")**: Indicate the positioning of design elements or features on the objects, which may be important for understanding the composition and iconography.==

- ==**Coin**: This term's prominence may reflect a significant numismatic collection, which could offer insights into economic history, trade, and cultural exchange.==

The prevalence of these terms suggests that the museum's collection may have a strong focus on detailed decorative arts and items that hold significant historical and aesthetic value. The presence of terms related to colors and design elements underscores the visual impact of these objects and their importance in conveying cultural artistry.

### Conclusion

---

Wrapping up, the Harvard Art Museum's API gave us a unique peek into how art from different cultures is collected and displayed. We saw lots of art, with a special focus on decorative details like "floral" and "patterns," and practical items like "coins." It's clear that the museum values these pieces, but the actual reasons why some things get more spotlight than others aren't always clear.

The word clouds were handy. They showed us what kind of items from Ottoman, Islamic, and Turkish collections are in the museum and how often they're viewed. It turns out Design Descriptions pops up a lot, which isn't too surprising given its importance in art.

Comparing all this to other museums would be interesting, to see if their collection of the same cultures represents the same ideas. All in all, this dive into the museum's data was pretty eye-opening. It showed us not just what's on display, but also how and why certain pieces might get more attention than others.

### **_Ready for grading_**

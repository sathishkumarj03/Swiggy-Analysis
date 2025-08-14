# Swiggy-Analysis
# 🍽️ Swiggy Restaurant Analysis – Power BI Dashboard

## 📊 Dashboard Overview
This Power BI project analyzes Swiggy restaurant data to extract key business insights such as top-performing cities, cuisine popularity, delivery efficiency, and customer satisfaction trends.  

Dataset: `ZOMATO`

---

## **Chart 1 – Restaurant Count and Average Rating by Area**
- **Chart Type:** Clustered Column Chart  
- **Dataset:** `ZOMATO`  
- **X-Axis:** `Area`  
- **Column Values:** 🆕 `Restaurant Count = COUNTROWS('ZOMATO')`  
- **Secondary Values:** `Average of Avg ratings` *(line shown as reference, not combo chart)*  
- **Tooltip:** `City`, `Total ratings`  
- **Insight:** Identifies areas with the most restaurants and compares their average ratings to spot quality hot zones.

---

## **Chart 2 – Count of City and Average Delivery Time by Food Type**
- **Chart Type:** Clustered Column Chart  
- **Dataset:** `ZOMATO`  
- **X-Axis:** `Food type` *(split into rows in Power Query)*  
- **Column Values:** `Count of City`  
- **Secondary Values:** `Average Delivery Time` *(as reference line)*  
- **Tooltip:** `Avg ratings`, `Price`  
- **Insight:** Shows which cuisines are most available across cities and their average delivery performance.

---

## **Chart 3 – Top Rated % by City and Total Restaurants by City**
- **Chart Type:** Clustered Column Chart  
- **Dataset:** `ZOMATO`  
- **X-Axis:** `City`  
- **Column Values:** `Total Restaurants`  
- **Secondary Values:** 🆕 `Top Rated % by City` *(percentage reference line)*  
- **Key DAX:**  
```dax
Top Rated % by City =
VAR Total =
    CALCULATE(COUNTROWS('ZOMATO'), ALLEXCEPT('ZOMATO', 'ZOMATO'[City]))
VAR TopRated =
    CALCULATE(
        COUNTROWS('ZOMATO'),
        FILTER('ZOMATO', 'ZOMATO'[Avg ratings] > 4.5),
        ALLEXCEPT('ZOMATO', 'ZOMATO'[City])
    )
RETURN
DIVIDE(TopRated, Total, 0)
```  
- **Insight:** Compares total restaurants and top-rated percentage per city.

---

## **Chart 4 – Top Rated Restaurants by Area**
- **Chart Type:** Clustered Column Chart  
- **Dataset:** `ZOMATO`  
- **X-Axis:** `Area`  
- **Y-Axis:** 🆕 `Top Rated Restaurants`  
```dax
Top Rated Restaurants =
CALCULATE(
    COUNTROWS('ZOMATO'),
    FILTER('ZOMATO', 'ZOMATO'[Avg ratings] > 4.5)
)
```  
- **Tooltip:** `City`, `Avg ratings`  
- **Insight:** Shows which areas have the highest count of highly rated restaurants.

---

## **Chart 5 – Top Rated % by City by Price Category**
- **Chart Type:** Clustered Column Chart  
- **Dataset:** `ZOMATO`  
- **X-Axis:** `City`  
- **Legend:** 🆕 `Price Category`  
```dax
Price Category =
SWITCH(
    TRUE(),
    'ZOMATO'[Price] <= 200, "Low (<= ₹200)",
    'ZOMATO'[Price] <= 500, "Mid (₹201–₹500)",
    'ZOMATO'[Price] <= 800, "High (₹501–₹800)",
    "Premium (> ₹800)"
)
```  
- **Values:** `Top Rated % by City`  
- **Insight:** Reveals how pricing tiers influence the share of top-rated restaurants per city.

---

## **Chart 6 – Average Rating and Average Delivery Time by Delivery Time Bucket**
- **Chart Type:** Clustered Column Chart  
- **Dataset:** `ZOMATO`  
- **X-Axis:** 🆕 `Delivery Time Bucket`  
```dax
Delivery Time Bucket =
SWITCH(
    TRUE(),
    'ZOMATO'[Delivery time] <= 30, "≤30 min",
    'ZOMATO'[Delivery time] <= 45, "31–45 min",
    'ZOMATO'[Delivery time] <= 60, "46–60 min",
    "> 60 min"
)
```  
- **Values:** `Avg Rating`  
```dax
Avg Rating = AVERAGE('ZOMATO'[Avg ratings])
```  
- **Insight:** Highlights the effect of delivery speed on ratings.

---

## **Chart 7 – Total Restaurants and Average Ratings by City**
- **Chart Type:** Clustered Column Chart  
- **Dataset:** `ZOMATO`  
- **X-Axis:** `City`  
- **Column Values:** `Total Restaurants`  
- **Secondary Values:** `Average of Avg ratings` *(as line)*  
- **Insight:** Identifies cities with the highest restaurant presence and compares average quality.

---

## **Chart 8 – Restaurant Count by Rating Category**
- **Chart Type:** Column Chart  
- **Dataset:** `ZOMATO`  
- **X-Axis:** 🆕 `Rating Category`  
```dax
Rating Category =
SWITCH(
    TRUE(),
    'ZOMATO'[Avg ratings] < 3, "Poor (<3)",
    'ZOMATO'[Avg ratings] < 4, "Average (3–3.9)",
    'ZOMATO'[Avg ratings] < 4.5, "Good (4–4.4)",
    "Excellent (4.5+)"
)
```  
- **Values:** `Restaurant Count`  
```dax
Restaurant Count = COUNTROWS('ZOMATO')
```  
- **Insight:** Shows distribution of restaurants by rating tier.

---

## **Chart 9 – Average Delivery Time by Restaurant**
- **Chart Type:** Column Chart  
- **Dataset:** `ZOMATO`  
- **X-Axis:** `Restaurant`  
- **Values:** `Average Delivery Time`  
- **Insight:** Highlights restaurants with very fast or slow delivery, useful for operational review.

---

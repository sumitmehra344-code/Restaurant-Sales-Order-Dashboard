# Restaurant-Sales-Order-Dashboard
Restaurant Sales ;  Order Dashboard
                       ┌── 🇮🇹 Italian  (8 dishes | $12.00 – $25.00)
                           ├── 🥢 Asian    (8 dishes |  $5.00 – $19.25)
   Menu Catalog (32 items) ┼── 🌮 Mexican  (9 dishes |  $7.00 – $19.50)
                           └── 🍔 American (7 dishes |  $5.00 – $13.95)
#### Fast Casual / Shared📊 Key Insights & Business Findings1####
Revenue Concentration by CuisineItalian dishes account for the largest proportion of gross revenue, driven by high price points (e.g., Seafood Fettuccine at $25.00 and Meat Lasagna at $17.95).Asian cuisine represents the highest repeat-volume segment, propelled by staple favorites like Korean Beef Bowls and Pork Ramen.American cuisine acts as a weekday lunch anchor with short ticket prep times (Burgers, Hot Dogs, Fries).2. Menu Engineering Matrix (BCG Matrix)Categorizing menu items by sales volume and ticket contribution:                     HIGH POPULARITY
                           ▲
             PLOWHORSES    │      STARS
        (High Vol, Low $)  │ (High Vol, High $)
        ───────────────────┼───────────────────
         French Fries      │ Korean Beef Bowl
         Edamame           │ Steak Tacos
         Potstickers       │ Seafood Fettuccine
        ───────────────────┼───────────────────
              DOGS         │     PUZZLES
        (Low Vol, Low $)   │ (Low Vol, High $)
        ───────────────────┼───────────────────
         Veggie Burrito    │ Salmon Roll
         Eggplant Parmesan │ Shrimp Scampi
                           │
                           ▼
                     LOW POPULARITY
        ◄────── LOW PRICE ───┼─── HIGH PRICE ──────►
QuadrantDishesStrategic Recommendation⭐ StarsKorean Beef Bowl, Hamburger, Steak TacosMaintain consistent ingredient specs; feature in social/menu highlights.🧩 PuzzlesSeafood Fettuccine, Salmon RollUpsell via floor staff recommendations; bundle with high-margin appetizers.🐎 PlowhorsesFrench Fries, Edamame, PotstickersIncrease profitability through modest price increments (+$0.50 to +$1.00).🐶 DogsVegetarian Burrito, Mushroom RavioliEvaluate ingredient overlaps; consider seasonal rotation or recipe reform.3. Peak Dining Windows & Service RhythmLunch Surge (12:00 PM – 1:30 PM): Concentrated around American handhelds (Burgers, Tacos, Fries) where ticket-to-table turnaround speed is critical.Dinner Service (5:30 PM – 7:30 PM): Extended table dwell times, higher attachment rates for appetizers (Edamame, Potstickers), and premium Italian entrees.🗄️ Dataset Architecture & DictionaryThe schema links raw kitchen ticket lines to the central menu catalog:
    m.item_name,
    m.category,
    COUNT(o.order_details_id) AS total_orders,
    SUM(m.price) AS total_revenue
FROM order_details o
JOIN menu_items m 
    ON o.item_id = m.menu_item_id
GROUP BY m.item_name, m.category
ORDER BY total_revenue DESC
LIMIT 5;
SELECT 
    m.category AS cuisine,
    COUNT(o.order_details_id) AS total_dishes_sold,
    ROUND(SUM(m.price), 2) AS gross_sales,
    ROUND(AVG(m.price), 2) AS avg_item_price
FROM order_details o
JOIN menu_items m 
    ON o.item_id = m.menu_item_id
GROUP BY m.category
ORDER BY gross_sales DESC;
SELECT 
    EXTRACT(HOUR FROM order_time) AS service_hour,
    COUNT(DISTINCT order_id) AS total_tickets,
    COUNT(order_details_id) AS total_items_ordered
FROM order_details
GROUP BY service_hour
ORDER BY total_tickets DESC;
💡 Actionable Recommendations for Restaurant ManagementPre-Rush Prep Alignment:Pre-portion high-volume appetizers (Edamame, Potstickers) before the 12:00 PM and 5:30 PM service windows to reduce ticket fulfillment times.Strategic Combo Bundling:Create fixed-price combinations (e.g., Korean Beef Bowl + Potstickers or Burger + Fries) to convert standalone diners into higher-margin multi-item tickets.Inventory & Menu Re-engineering:Review bottom-decile items that utilize single-use ingredients. Consolidate or sunset them to simplify line cook training and reduce cold-storage spoilage.🚀 Quickstart & Repository StructureFolder Layout├── data/
│   ├── menu_items.csv          # Catalog of dishes, cuisines, and prices
│   └── order_details.csv       # Granular transaction line-item records
├── Resturat_Dashboard.pbix     # Interactive Power BI workbook
├── sql/
│   └── restaurant_queries.sql  # Extraction & transformation queries
└── README.md                   # Project documentation

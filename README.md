# Project name here
> Summary description here.


This file will become your README and also the index of your documentation.

## Install

`pip install your_project_name`

## Usage

very simple usage, 
init the search object then call sortedSearch()


```python
from cloudsearch.cloudsearch import Search
searchEndpoint = 'https://search-villa-cloudsearch-2-4izacsoytzqf6kztcyjhssy2ti.ap-southeast-1.cloudsearch.amazonaws.com'
searcher = Search(searchTerm = 'banana', key=USER, pw= PW , endpoint=searchEndpoint)
result = searcher.search(size=1000)
print(f'found {len(list(result))} results, the first item is \n{next(iter(result))}')
```

    found 464 results, the first item is 
    {'pr_country_en': 'Thailand', 'pr_abb': 'BANANA', 'pr_engname': 'BANANA', 'pr_name': 'BANANA', 'pr_online_name_en': 'BANANA', 'pr_keyword_en': 'banana,bananas,Cavendish Banana,Cavendish,,Fresh fruit', 'pr_online_name_th': 'BANANA', 'cprcode': '0226238', 'villa_category_l2_en': 'Fresh Produce', 'content_en': '0226238 BANANA banana,bananas,Cavendish Banana,Cavendish,,Fresh fruit', 'pr_name_th': 'BANANA', 'iprcode': '0226238', 'oprcode': '0226238', 'villa_category_l3_en': 'Fruits & Vegetable Local', 'hema_name_en': 'BANANA', 'pr_keyword_th': 'กล้วย,กล้วยหอม,กล้วยไข่,กล้วยออร์แกนิก,กล้วออร์แกนิค,Fresh fruit', 'hema_name_th': 'BANANA', 'pr_name_en': 'BANANA', 'pr_code': '0226238', 'villa_category_l4_en': 'Local Fruit', 'content_th': 'BANANA กล้วย,กล้วยหอม,กล้วยไข่,กล้วยออร์แกนิก,กล้วออร์แกนิค,Fresh fruit', 'pr_active': 'Y', 'villa_category_l1_en': 'Fresh'}


## For a more complex requirement
You can extend the class, please have a look at sortedSearch example

```python
_ = searcher.sortedSearch()
```

    raw search result is     villa_category_l1_en villa_category_l2_en      villa_category_l3_en  \
    0                  Fresh        Fresh Produce  Fruits & Vegetable Local   
    1                    NaN                  NaN                       NaN   
    2                  Fresh        Fresh Produce                    Bakery   
    3                  Fresh        Fresh Produce                    Bakery   
    4                  Fresh        Fresh Produce                    Bakery   
    ..                   ...                  ...                       ...   
    459                Fresh        Seafood Fresh                Crustacean   
    460                Fresh        Seafood Fresh                Crustacean   
    461                Fresh        Seafood Fresh             Local Seafood   
    462                Fresh               Frozen                   Seafood   
    463                Fresh        Seafood Fresh             Local Seafood   
    
        villa_category_l4_en  
    0            Local Fruit  
    1                    NaN  
    2          Cake category  
    3          Cake category  
    4         Pasty category  
    ..                   ...  
    459                Local  
    460                Local  
    461                  NaN  
    462                  NaN  
    463                  NaN  
    
    [464 rows x 4 columns]


```python
import json
boostDict = {
    "fields":["online_category_l1_en"]
}
queryOptions = json.dumps(boostDict)
searcher2 = Search(searchTerm = 'dairy', key=USER, pw= PW , endpoint=searchEndpoint)
result = searcher2.sortedSearch(queryOptions = queryOptions, size=10)
```

    raw search result is   villa_category_l1_en villa_category_l2_en villa_category_l3_en  \
    0                Fresh                Dairy     Chilled Beverage   
    1                Fresh                Dairy                 Milk   
    2                Fresh                Dairy               Cheese   
    3                Fresh                Dairy     Chilled Beverage   
    4                Fresh                Dairy                 Milk   
    5                  NaN                  NaN                  NaN   
    6                  NaN                  NaN                  NaN   
    7                Fresh                Dairy               Cheese   
    8                Fresh                Dairy               Yogurt   
    9                Fresh                Dairy               Yogurt   
    
      villa_category_l4_en  
    0                  NaN  
    1                  NaN  
    2       Cheddar Cheese  
    3                  NaN  
    4                  NaN  
    5                  NaN  
    6                  NaN  
    7                  NaN  
    8               Yogurt  
    9        YogurtGranola  


```python
filterQuery = "(or " + " ".join(['online_category_l1_en' + f":'{cat}'" for cat in ['Dairy']]) + ")"
searcher3 = Search(searchTerm = 'dairy', key=USER, pw= PW , endpoint=searchEndpoint)
result = searcher3.sortedSearch(filterQuery = filterQuery, size=10)
```

    raw search result is   villa_category_l1_en villa_category_l2_en villa_category_l3_en  \
    0                  NaN                  NaN                  NaN   
    1                Fresh                Dairy                 Milk   
    2                Fresh                Dairy                Cream   
    3                Fresh                Dairy                Cream   
    4                Fresh                Dairy               Yogurt   
    5                Fresh                Dairy               Yogurt   
    6                Fresh                Dairy               Yogurt   
    7                Fresh                Dairy               Yogurt   
    8                Fresh                Dairy               Yogurt   
    9                Fresh                Dairy               Yogurt   
    
       villa_category_l4_en  
    0                   NaN  
    1                   NaN  
    2                   NaN  
    3                   NaN  
    4                   NaN  
    5                   NaN  
    6                   NaN  
    7                   NaN  
    8                   NaN  
    9                   NaN  


```python
result[9]['online_category_l1_en']
```




    'Dairy'



# Movies-ETL
## Overview
Create code to perform ETL process (Extract, Transform, Load). The below three files of data contain movie data.
Using the ETL process, able to extract and clean the data in order to push to database tables in order to run SQL queries.
- Wikipedia data
- Kaggle metadata
- MovieLens rating data

## Results
Cleaning the wiki data created the below data frame with the below columns:

![image](https://user-images.githubusercontent.com/90691846/140616280-7a5494d7-e9e5-4653-86aa-ad964992233e.png)

![image](https://user-images.githubusercontent.com/90691846/140616298-a4e0fb95-dd5d-4113-986e-0be912ce1277.png)

The clean kaggle data created the 3 data frames below:

![image](https://user-images.githubusercontent.com/90691846/140616339-84d28d7e-ced0-4f6b-8eb5-436ae97337f8.png)

![image](https://user-images.githubusercontent.com/90691846/140616353-f86e75c5-281f-40a7-91d9-39e817c2cdcd.png)

![image](https://user-images.githubusercontent.com/90691846/140616370-98c56f44-0e2d-4e61-a8c1-f1034bd46673.png)

## Summary
After successfully cleaning the data I was able to merge the data frames in order to load the data into a database.
This was accomplished with using user defined functions along with regex expressions. Some of the sample code is as
follows:

```
 # Add the parse_dollars function.
    def parse_dollars(s):
        # if s is not a string, return NaN
        if type(s) != str:
            return np.nan
    
        # if input is of the form $###.# million
        if re.match(r'\$\s*\d+\.?\d*\s*milli?on', s, flags=re.IGNORECASE):
            # remove dollar sign and " million"
            s = re.sub('\$|\s|[a-zA-Z]','',s)
            # convert to float and multiply by a million
            value = float(s) * 10**6
            # return value
            return value
    
        # if input is of the form $$###.# billion
        elif re.match(r'\$\s*\d+\.?\d*\s*billi?on', s, flags=re.IGNORECASE):
            # remove dollar sign and " billion"
            s = re.sub('\$|\s|[a-zA-Z]','', s)
            # convert to float and multiply by a billion
            value = float(s) * 10**9
            # return value
            return value
    
        # if input is of the form $###,###,###
        elif re.match(r'\$\s*\d{1,3}(?:[,\.]\d{3})+(?!\s[mb]illion)', s, flags=re.IGNORECASE):    
            # remove dollar sign and commas
            s = re.sub('\$|,', '', s)
            # convert to float
            value = float(s)
            # return value
            return value
        
        # otherwise, return NaN
        else:
            return np.nan
```

Data was successfully loaded into the database into 2 distinct tables, movies and ratings:

![image](https://user-images.githubusercontent.com/90691846/140616818-a10979ce-2e55-42d9-b09e-b9a879900f5e.png)

![image](https://user-images.githubusercontent.com/90691846/140616834-38fcfa45-f8ad-498b-b1ca-7f82847f1553.png)


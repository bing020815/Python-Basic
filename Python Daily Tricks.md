# Pandas/Matplotlib Tricks

## Plots Configuration:
* plt.figure(figsize=(10,8))
* plot(figsize=(), title=)

## magic command
* %matplotlib inline  
* %%script false 

## For loop:
* .iterrows() - Iterate over DataFrame rows as (index, Series) pairs
* .itertuples() - Iterate over DataFrame rows as namedtuples, with index value as first element of the tuple.
* .items() - Iterate over (column name, Series) pairs. Best for the dictionary
* .values() - return the value ( works for a dictionary)
* enumerate(iterable, start=0) - Return an enumerate object.
* reversed()
* sorted(, reverse=True)
* sorted(dictionary, key = lambda i: (i["id"], i["name"]), reverse=True )
* filter(lambda i: i["id"] % 2 == 0, students)
* zip() - to map the similar index of multiple containers so that they can be used just using as single entity

## Pandas Configuration:
* pd.set_option('display.max_rows', None)
* pd.set_option('display.max_columns', None)
* pd.set_option('display.width', None)
* pd.set_option('display.max_colwidth', None)
* pd.set_option('precision', 4)  -  sets the output display precision in terms of decimal places
* pd.set_option('chop_threshold', .5) - sets at a level that pandas rounds to zero (<0.5 becomes 0). It does not change the precision at which the number is stored.

#### Set maximum rows display at 500
* pd.set_option('display.max_rows', 500)
* pd.set_option('display.max_columns', 50)
* pd.set_option('display.max_colwidth', 1000)
* pd.reset_option('display.max_colwidth')

## Pandas styling functions
* pd.set_option('colheader_justify', 'left')
* df.style.set_properties(subset=['column'], **{'text-align': 'left'})  - set the text align to the left
* df.style.set_properties(color="black", align="left",)
* df.style.set_properties(**{'background-color': 'yellow'})
* df.style.hide_index()
* df.style.hide_columns(['C','D'])
* df.head().style.format({"JobTitle": lambda x:x.lower(), "BasePay": "${:20,.0f}", "OtherPay": "${:20,.0f}", "TotalPay": "${:20,.0f}"})

#### missing value:
```
df.style
   .set_na_rep("FAIL")
   .format(None, na_rep="PASS", subset=["D"])
   .highlight_null("yellow"))
```

## Time series
* df = pd.read_csv('yourfile.csv',parse_dates=['date'])  or  df['datetime'] = pd.to_datetime(df['date'])
* df = df.set_index('date')
* df.index=pd.to_datetime(df.index)
* pandas.DataFrame.resample  --  df_ts[['column']].resample('M').count()

* pandas.Series.values - Return Series as ndarray or ndarray-like depending on the dtype
* .squeeze() - Squeeze 1 dimensional axis objects into scalars.
* pandas.DataFrame.size() - Return an int representing the number of elements in this object

* pandas.DataFrame.sample() - Returns a random sample of items from an axis of object.
* pandas.Series.between(self, left, right, inclusive=True) - Return boolean Series equivalent to left <= series <= right
* pandas.DataFrame.between_time(start_time, end_time, include_start= True, include_end = True, axis=None) - Select values between particular times of the day


## Data manipulation:
* pandas.DataFrame.drop(columns=None, inplace=False) - Drop specified labels from rows or columns.
* pandas.DataFrame.dropna(inplace=False) - Remove missing values
* andas.DataFrame.fillna(value=None, inplace=False) - Fill NA/NaN values using the specified method.
* pandas.notnull() - Detect non-missing values for an array-like object.
* pandas.isnull() - Detect missing values for an array-like object.
* pandas.Series.str.zfill(self, width) - Pad strings in the Series/Index by prepending ‘0’ characters.
* pandas.DataFrame.sort_values(by, axis=0, ascending=True, inplace=False, ignore_index=False)
* pandas.DataFrame.drop_duplicates(inplace: bool = False) - Return DataFrame with duplicate rows removed.
* DataFrame.unique(self, axis=0, dropna=True) - Count distinct observations over requested axis.
* dataTypeSeries.dtypes
* pandas.Series.value_counts
* pandas.assign() - assign new columns to a DataFrame, returning a new object (a copy) with the new columns added to the originmal ones
* pandas.DataFrame.iloc[] - Purely integer-location based indexing for selection by position
* pandas.DataFrame.loc[] - Access a group of rows and columns by label(s) or a boolean array.
* pandas.DataFrame.mode(dropna=True) - Get the mode(s) of each element along the selected axis.
* pandas.Series.unique() - Return unique values of Series object.
* pandas.DataFrame[pandas.Series.str.contains(" words ")] - wildcard
* pandas.DataFrame.set_index() - Set the DataFrame index using existing columns.
* pandas.DataFrame.reset_index() - Reset the index, or a level of it
* pandas.DataFrame.rename(index=index_mapper, columns=columns_mapper, ...) - Alter axes labels.
* df = df.astype({'Year':str}) 
* pandas.concat(axis{0/’index’, 1/’columns’}, default 0) - Concatenate pandas objects along a particular axis with optional set logic along the other axes.


## NA:
* DataFrame.fillna()


## Groupby:
* df.groupby(['column'], as_index=False).first() - show the df after groupby without computation (split -> combine)
* df.groupby(['column'], as_index=False).agg({"other column": "any computation"})


## Big data:
```
chunks = pd.read_csv('file path://', parse_dates=['Date'], chunksize=100000, low_memory=False)  # the number of rows per chunk
# append chunk to a list
dfList = []
for chunk in chunks:
    dfList.append(chunk)
# convert to dataframe
df = pd.concat(dfList, sort=False)
```


# STRINGS
## properties: iterable, immutable
### create a string
* s = str(42)         # convert another data type into a string
* s = 'I like you'

### examine a string
* s[0]                # returns 'I'
* len(s)              # returns 10

### string slicing is like list slicing
* s[:6]               # returns 'I like'
* s[7:]               # returns 'you'
* s[-1]               # returns 'u'

### basic string methods (does not modify the original string)
* s.lower()           # returns 'i like you'
* s.upper()           # returns 'I LIKE YOU'
* s.startswith('I')   # returns True
* s.endswith('you')   # returns True
* s.isdigit()         # returns False (returns True if every character in the string is a digit)
* s.find('like')      # returns index of first occurrence (2), but doesn't support regex
* s.find('hate')      # returns -1 since not found
* s.rfind(t)            # index of last instance of string t inside s (-1 if not found)
* s.replace('like', 'love')    # replaces all instances of 'like' with 'love'
* s.title()               # a titlecased version of the string s
* s.index(t)          # like s.find(t) except it raises ValueError if not found
* s.rindex(t)        # like s.rfind(t) except it raises ValueError if not found


### split a string into a list of substrings separated by a delimiter
* s.split(' ')        # returns ['I', 'like', 'you']
* s.split()           # equivalent (since space is the default delimiter)
* s2 = 'a, an, the'
* s2.split(',')       # returns ['a', ' an', ' the']
* s.splitlines()     # split s into a list of strings, one per line

### join a list of strings into one string using a delimiter
* stooges = ['larry', 'curly', 'moe']
* ' '.join(stooges)   # returns 'larry curly moe'

### concatenate strings
* s3 = 'The meaning of life is'
* s4 = '42'
* s3 + ' ' + s4       # returns 'The meaning of life is 42'

### remove whitespace from start and end of a string
* s5 = '  ham and cheese  '
* s5.strip()          # returns 'ham and cheese'

### string substitutions: all of these return 'raining cats and dogs'
* 'raining %s and %s' % ('cats', 'dogs')                       # old way
* 'raining {} and {}'.format('cats', 'dogs')                   # new way
* 'raining {arg1} and {arg2}'.format(arg1='cats', arg2='dogs') # named arguments

### string formatting
*  more examples: https://mkaz.blog/code/python-string-format-cookbook/
* 'pi is {:.2f}'.format(3.14159)      # returns 'pi is 3.14'

### normal strings versus raw strings
```
print('first line\nsecond line')    # normal strings allow for escaped characters
print(r'first line\nfirst line')    # raw strings treat backslashes as literal characters
```

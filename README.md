cortanaanalytics
================

This a Python library for using Microsoft Azure Datamarket and Cortana Analytics Services.


Installation
------------

To install, use pip:

```
pip install cortanaanalytics
```

You can also get the development versions directly from the GitHub repo: http://github.com/crwilcox/cortanaanalytics

Getting Started
---------------
Cortana Analytics has many different packages. Please look at each section for the library you are interested in.

Also, you will need [obtain an access key](https://datamarket.azure.com/account/keys) from the Azure Datamarket and subscribe to the service you wish to use.	

Text Analytics
--------------
https://datamarket.azure.com/dataset/amla/text-analytics

```python

from cortanaanalytics.textanalytics import TextAnalytics

key = '1abCdEFGh/ijKlmN/opq234r56st/UvWXYZabCD7EF8='
ta = TextAnalytics(key)

score = ta.get_sentiment("hello world")

scores = ta.get_sentiment_batch([{"Text":"hello world", "Id":0}, {"Text":"hello world again", "Id":2}])
```

Recommendations
---------------
https://datamarket.azure.com/dataset/amla/recommendations

```python
from cortanaanalytics.recommendations import Recommendations

email = 'email@outlook.com'
key = '1abCdEFGh/ijKlmN/opq234r56st/UvWXYZabCD7EF8='
rs = Recommendations(email, key)

# create model
model_id = rs.create_model('groceries' + datetime.now().strftime('%Y%m%d%H%M%S'))

# import item catalog
catalog_path = os.path.join('app', 'management', 'commands', 'catalog.csv')
rs.import_file(model_id, catalog_path, Uris.import_catalog)

# import usage information
transactions_path = os.path.join('app', 'management', 'commands', 'transactions.csv')
rs.import_file(model_id, transactions_path, Uris.import_usage)

# build model
build_id = rs.build_fbt_model(model_id)
status = rs.wait_for_build(model_id, build_id)

if status != BuildStatus.success:
    print('Unsuccessful in building the model, failing now.')
    return

# update model active build (not needed unless you are rebuilding)
rs.update_model(model_id, None, build_id)

print('Built a model. Model ID:{} Build ID:{}'.format(model_id, build_id))
```

# Kafka Producer

### Libraries Used:
##### pandas (p): For data manipulation and analysis.
##### kafka (KafkaConsumer, KafkaProducer): To consume and produce messages to/from Kafka topics.
##### time (sleep): To pause the execution for a specified amount of time.
##### json (dumps): To handle JSON data.
##### s3fs: For accessing Amazon S3 as if it were a local file system, allowing direct interaction with S3 buckets.


```python
!pip install kafka-python
```

    Collecting kafka-python
      Downloading kafka_python-2.0.2-py2.py3-none-any.whl.metadata (7.8 kB)
    Downloading kafka_python-2.0.2-py2.py3-none-any.whl (246 kB)
       ---------------------------------------- 0.0/246.5 kB ? eta -:--:--
       ---- ---------------------------------- 30.7/246.5 kB 445.2 kB/s eta 0:00:01
       ------------ -------------------------- 81.9/246.5 kB 919.0 kB/s eta 0:00:01
       -------------------- ----------------- 133.1/246.5 kB 983.0 kB/s eta 0:00:01
       ------------------------- ------------ 163.8/246.5 kB 821.4 kB/s eta 0:00:01
       --------------------------------- ---- 215.0/246.5 kB 819.2 kB/s eta 0:00:01
       -------------------------------------  245.8/246.5 kB 838.1 kB/s eta 0:00:01
       -------------------------------------  245.8/246.5 kB 838.1 kB/s eta 0:00:01
       -------------------------------------- 246.5/246.5 kB 605.8 kB/s eta 0:00:00
    Installing collected packages: kafka-python
    Successfully installed kafka-python-2.0.2
    


```python
import pandas as p
from kafka import KafkaConsumer, KafkaProducer
from time import sleep
from json import dumps
import json
```


```python
producer = KafkaProducer(bootstrap_servers = ['Your IP Address:9092'],
                         value_serializer = lambda x:
                         dumps(x).encode('utf-8'))
```


```python
producer.send('demo_test', value = "{'hello':'Izhan'}")
```




    <kafka.producer.future.FutureRecordMetadata at 0x1632749f150>




```python
df = p.read_csv("indexProcessed.csv")
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Index</th>
      <th>Date</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Adj Close</th>
      <th>Volume</th>
      <th>CloseUSD</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>HSI</td>
      <td>1986-12-31</td>
      <td>2568.300049</td>
      <td>2568.300049</td>
      <td>2568.300049</td>
      <td>2568.300049</td>
      <td>2568.300049</td>
      <td>0.0</td>
      <td>333.879006</td>
    </tr>
    <tr>
      <th>1</th>
      <td>HSI</td>
      <td>1987-01-02</td>
      <td>2540.100098</td>
      <td>2540.100098</td>
      <td>2540.100098</td>
      <td>2540.100098</td>
      <td>2540.100098</td>
      <td>0.0</td>
      <td>330.213013</td>
    </tr>
    <tr>
      <th>2</th>
      <td>HSI</td>
      <td>1987-01-05</td>
      <td>2552.399902</td>
      <td>2552.399902</td>
      <td>2552.399902</td>
      <td>2552.399902</td>
      <td>2552.399902</td>
      <td>0.0</td>
      <td>331.811987</td>
    </tr>
    <tr>
      <th>3</th>
      <td>HSI</td>
      <td>1987-01-06</td>
      <td>2583.899902</td>
      <td>2583.899902</td>
      <td>2583.899902</td>
      <td>2583.899902</td>
      <td>2583.899902</td>
      <td>0.0</td>
      <td>335.906987</td>
    </tr>
    <tr>
      <th>4</th>
      <td>HSI</td>
      <td>1987-01-07</td>
      <td>2607.100098</td>
      <td>2607.100098</td>
      <td>2607.100098</td>
      <td>2607.100098</td>
      <td>2607.100098</td>
      <td>0.0</td>
      <td>338.923013</td>
    </tr>
  </tbody>
</table>
</div>




```python
while True:
    dict_stock = df.sample(1).to_dict(orient="records")[0]
    producer.send('demo_test', value=dict_stock)
```


    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    Cell In[9], line 2
          1 while True:
    ----> 2     dict_stock = df.sample(1).to_dict(orient="records")[0]
          3     producer.send('demo_test', value=dict_stock)
    

    File C:\Python311\Lib\site-packages\pandas\core\generic.py:5858, in NDFrame.sample(self, n, frac, replace, weights, random_state, axis, ignore_index)
       5855 if weights is not None:
       5856     weights = sample.preprocess_weights(self, weights, axis)
    -> 5858 sampled_indices = sample.sample(obj_len, size, replace, weights, rs)
       5859 result = self.take(sampled_indices, axis=axis)
       5861 if ignore_index:
    

    File C:\Python311\Lib\site-packages\pandas\core\sample.py:151, in sample(obj_len, size, replace, weights, random_state)
        148     else:
        149         raise ValueError("Invalid weights: weights sum to zero")
    --> 151 return random_state.choice(obj_len, size=size, replace=replace, p=weights).astype(
        152     np.intp, copy=False
        153 )
    

    KeyboardInterrupt: 


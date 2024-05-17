# Kafka Consumer

### Libraries Used:
##### kafka (KafkaConsumer): To consume messages from a Kafka topic.
##### json (loads, dumps): To handle JSON data.
##### boto3: To interact with Amazon Web Services (AWS), specifically for uploading data to an S3 bucket.


```python
from kafka import KafkaConsumer
from json import loads
import boto3
import json

# Set AWS credentials (optional if already configured in the environment)
# boto3.setup_default_session(aws_access_key_id='your_access_key_id', aws_secret_access_key='your_secret_access_key', region_name='ap-south-1')

# Initialize Kafka consumer
consumer = KafkaConsumer(
    'demo_test',
    bootstrap_servers=['Your IP Address:9092'],
    value_deserializer=lambda x: loads(x.decode('utf-8'))
)

# Initialize S3 client
s3 = boto3.client('s3')

# Iterate over messages from Kafka and upload to S3
for count, message in enumerate(consumer):
    # Construct S3 file path
    s3_path = f"kafka-stock-market-project-izhan/stock_market_{count}.json"
    # Upload the message value as JSON to S3
    s3.put_object(Bucket='kafka-stock-market-project-izhan', Key=s3_path, Body=json.dumps(message.value))

# Close Kafka consumer after processing messages
# consumer.close()

```


    ---------------------------------------------------------------------------

    KeyboardInterrupt                         Traceback (most recent call last)

    Cell In[8], line 24
         22     s3_path = f"kafka-stock-market-project-izhan/stock_market_{count}.json"
         23     # Upload the message value as JSON to S3
    ---> 24     s3.put_object(Bucket='kafka-stock-market-project-izhan', Key=s3_path, Body=json.dumps(message.value))
         26 # Close Kafka consumer after processing messages
         27 # consumer.close()
    

    File C:\Python311\Lib\site-packages\botocore\client.py:553, in _api_call(self, *args, **kwargs)
        550         mapping[py_operation_name] = operation_name
        551     return mapping
    --> 553 def _create_api_method(
        554     self, py_operation_name, operation_name, service_model
        555 ):
        556     def _api_call(self, *args, **kwargs):
        557         # We're accepting *args so that we can give a more helpful
        558         # error message than TypeError: _api_call takes exactly
        559         # 1 argument.
        560         if args:
    

    File C:\Python311\Lib\site-packages\botocore\client.py:989, in _make_api_call(self, operation_name, api_params)
        981 resolve_checksum_context(request_dict, operation_model, api_params)
        983 service_id = self._service_model.service_id.hyphenize()
        984 handler, event_response = self.meta.events.emit_until_response(
        985     'before-call.{service_id}.{operation_name}'.format(
        986         service_id=service_id, operation_name=operation_name
        987     ),
        988     model=operation_model,
    --> 989     params=request_dict,
        990     request_signer=self._request_signer,
        991     context=request_context,
        992 )
        994 if event_response is not None:
        995     http, parsed_response = event_response
    

    File C:\Python311\Lib\site-packages\botocore\client.py:1015, in _make_request(self, operation_model, request_dict, request_context)
       1001     http, parsed_response = self._make_request(
       1002         operation_model, request_dict, request_context
       1003     )
       1005 self.meta.events.emit(
       1006     'after-call.{service_id}.{operation_name}'.format(
       1007         service_id=service_id, operation_name=operation_name
       (...)
       1012     context=request_context,
       1013 )
    -> 1015 if http.status_code >= 300:
       1016     error_info = parsed_response.get("Error", {})
       1017     error_code = error_info.get("QueryErrorCode") or error_info.get(
       1018         "Code"
       1019     )
    

    File C:\Python311\Lib\site-packages\botocore\endpoint.py:119, in Endpoint.make_request(self, operation_model, request_dict)
        113 def make_request(self, operation_model, request_dict):
        114     logger.debug(
        115         "Making request for %s with params: %s",
        116         operation_model,
        117         request_dict,
        118     )
    --> 119     return self._send_request(request_dict, operation_model)
    

    File C:\Python311\Lib\site-packages\botocore\endpoint.py:199, in Endpoint._send_request(self, request_dict, operation_model)
        197 self._update_retries_context(context, attempts)
        198 request = self.create_request(request_dict, operation_model)
    --> 199 success_response, exception = self._get_response(
        200     request, operation_model, context
        201 )
        202 while self._needs_retry(
        203     attempts,
        204     operation_model,
       (...)
        207     exception,
        208 ):
        209     attempts += 1
    

    File C:\Python311\Lib\site-packages\botocore\endpoint.py:241, in Endpoint._get_response(self, request, operation_model, context)
        235 def _get_response(self, request, operation_model, context):
        236     # This will return a tuple of (success_response, exception)
        237     # and success_response is itself a tuple of
        238     # (http_response, parsed_dict).
        239     # If an exception occurs then the success_response is None.
        240     # If no exception occurs then exception is None.
    --> 241     success_response, exception = self._do_get_response(
        242         request, operation_model, context
        243     )
        244     kwargs_to_emit = {
        245         'response_dict': None,
        246         'parsed_response': None,
        247         'context': context,
        248         'exception': exception,
        249     }
        250     if success_response is not None:
    

    File C:\Python311\Lib\site-packages\botocore\endpoint.py:281, in Endpoint._do_get_response(self, request, operation_model, context)
        279     http_response = first_non_none_response(responses)
        280     if http_response is None:
    --> 281         http_response = self._send(request)
        282 except HTTPClientError as e:
        283     return (None, e)
    

    File C:\Python311\Lib\site-packages\botocore\endpoint.py:377, in Endpoint._send(self, request)
        376 def _send(self, request):
    --> 377     return self.http_session.send(request)
    

    File C:\Python311\Lib\site-packages\botocore\httpsession.py:464, in URLLib3Session.send(self, request)
        461     conn.proxy_headers['host'] = host
        463 request_target = self._get_request_target(request.url, proxy_url)
    --> 464 urllib_response = conn.urlopen(
        465     method=request.method,
        466     url=request_target,
        467     body=request.body,
        468     headers=request.headers,
        469     retries=Retry(False),
        470     assert_same_host=False,
        471     preload_content=False,
        472     decode_content=False,
        473     chunked=self._chunked(request.headers),
        474 )
        476 http_response = botocore.awsrequest.AWSResponse(
        477     request.url,
        478     urllib_response.status,
        479     urllib_response.headers,
        480     urllib_response,
        481 )
        483 if not request.stream_output:
        484     # Cause the raw stream to be exhausted immediately. We do it
        485     # this way instead of using preload_content because
        486     # preload_content will never buffer chunked responses
    

    File C:\Python311\Lib\site-packages\urllib3\connectionpool.py:790, in HTTPConnectionPool.urlopen(self, method, url, body, headers, retries, redirect, assert_same_host, timeout, pool_timeout, release_conn, chunked, body_pos, preload_content, decode_content, **response_kw)
        787 response_conn = conn if not release_conn else None
        789 # Make the request on the HTTPConnection object
    --> 790 response = self._make_request(
        791     conn,
        792     method,
        793     url,
        794     timeout=timeout_obj,
        795     body=body,
        796     headers=headers,
        797     chunked=chunked,
        798     retries=retries,
        799     response_conn=response_conn,
        800     preload_content=preload_content,
        801     decode_content=decode_content,
        802     **response_kw,
        803 )
        805 # Everything went great!
        806 clean_exit = True
    

    File C:\Python311\Lib\site-packages\urllib3\connectionpool.py:536, in HTTPConnectionPool._make_request(self, conn, method, url, body, headers, retries, timeout, chunked, response_conn, preload_content, decode_content, enforce_content_length)
        534 # Receive the response from the server
        535 try:
    --> 536     response = conn.getresponse()
        537 except (BaseSSLError, OSError) as e:
        538     self._raise_timeout(err=e, url=url, timeout_value=read_timeout)
    

    File C:\Python311\Lib\site-packages\urllib3\connection.py:461, in HTTPConnection.getresponse(self)
        458 from .response import HTTPResponse
        460 # Get the response from http.client.HTTPConnection
    --> 461 httplib_response = super().getresponse()
        463 try:
        464     assert_header_parsing(httplib_response.msg)
    

    File C:\Python311\Lib\http\client.py:1378, in HTTPConnection.getresponse(self)
       1376 try:
       1377     try:
    -> 1378         response.begin()
       1379     except ConnectionError:
       1380         self.close()
    

    File C:\Python311\Lib\http\client.py:318, in HTTPResponse.begin(self)
        316 # read until we get a non-100 response
        317 while True:
    --> 318     version, status, reason = self._read_status()
        319     if status != CONTINUE:
        320         break
    

    File C:\Python311\Lib\http\client.py:279, in HTTPResponse._read_status(self)
        278 def _read_status(self):
    --> 279     line = str(self.fp.readline(_MAXLINE + 1), "iso-8859-1")
        280     if len(line) > _MAXLINE:
        281         raise LineTooLong("status line")
    

    File C:\Python311\Lib\socket.py:706, in SocketIO.readinto(self, b)
        704 while True:
        705     try:
    --> 706         return self._sock.recv_into(b)
        707     except timeout:
        708         self._timeout_occurred = True
    

    File C:\Python311\Lib\ssl.py:1278, in SSLSocket.recv_into(self, buffer, nbytes, flags)
       1274     if flags != 0:
       1275         raise ValueError(
       1276           "non-zero flags not allowed in calls to recv_into() on %s" %
       1277           self.__class__)
    -> 1278     return self.read(nbytes, buffer)
       1279 else:
       1280     return super().recv_into(buffer, nbytes, flags)
    

    File C:\Python311\Lib\ssl.py:1134, in SSLSocket.read(self, len, buffer)
       1132 try:
       1133     if buffer is not None:
    -> 1134         return self._sslobj.read(len, buffer)
       1135     else:
       1136         return self._sslobj.read(len)
    

    KeyboardInterrupt: 


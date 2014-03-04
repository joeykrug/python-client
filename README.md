©2014 BITPAY, INC.

Permission is hereby granted to any person obtaining a copy of this software
and associated documentation for use and/or modification in association with
the bitpay.com service.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.


Bitcoin Python payment library using the bitpay.com service.


Installation
------------
Varies depending on what python webserver framework you are using (SimpleHTTPServer, Flask, Bottle, Django, etc.).
Most of the calls will run as-is from a script, but bpVerifyNotification will require updating depending on your framework.
You can integrate these functions into your custom shopping cart implementation.


Configuration
-------------
Note: Python 2.7 and its built in libraries are required for use of this code library.

1. Create an API key at bitpay.com by clicking My Account > API Access Keys > Add New API Key.
2. In the bp_options.py file, configure the options specific to your implementation.


Usage
-----
1. In your shopping cart code, call bpCreateInvoice() with the appropriate orderID, price,
   posData and options.
2. The library will attempt to POST the new invoice information via curl to the BitPay
   network.  If successful, you will receive an invoice in the return response.  Any errors
   in this process will return an array with a single element: 'error' and the exception msg.
3. You may use the bpLog function manually to log any information you would like to track or
   automatically by setting the useLogging option to true in the bp_options file.  The log file
   could potentially get very large, depending on usage, so monitor closely or only use
   during debugging.
4. Responses from the BitPay network are JSON. You can use the new decodeResponse() function to
   convert these to an associative array, if needed.


Troubleshooting
---------------
The official BitPay API documentation should always be your first reference for development:
https://bitpay.com/downloads/bitpayApi.pdf

1. Verify that your "notificationURL" for the invoice is "https://" (not "http://")
2. Ensure a valid SSL certificate is installed on your server. Also ensure your root CA cert is
   updated. If your CA cert is not current, you will see curl SSL verification errors.
3. Verify that your callback handler at the "notificationURL" is properly receiving POSTs. You
   can verify this by POSTing your own messages to the server from a tool like Chrome Postman.
4. Verify that the POST data received is properly parsed and that the logic that updates the
   order status on the merchants web server is correct.
5. Verify that the merchants web server is not blocking POSTs from servers it may not
   recognize. Double check this on the firewall as well, if one is being used.
6. Use the logging functionality to log errors during development. If you contact BitPay support,
   they will ask to see the log file to help diagnose any problems.
7. Check the version of this Python library agains the official repository to ensure you are using
   the latest version. Your issue might have been addressed in a newer version of the library.
8. If all else fails, send an email describing your issue *in detail* to support@bitpay.com
9. To manually test the library, update the options file with your API key and do the following:
<pre>
$ python
>>> import bp_lib
>>> bp_lib.bpCreateInvoice(123, 1, 'fish')
</pre>
After a brief pause, you should get a JSON response that looks something like:
<pre>
{u'status': u'new', u'invoiceTime': 1393950046292, u'currentTime': 1393950046520, u'url': u'https://bitpay.com/invoice?id=aASDF2jh4ashkASDfh234', u'price': 1, u'btcPrice': u'1.0000', u'currency': u'BTC', u'posData': u'{"posData": "fish", "hash": "ASDfkjha452345ASDFaaskjhasdlfkflkajsdf"}', u'expirationTime': 1393950946292, u'id': u'aASDF2jh4ashkASDfh234'}
</pre>

Change Log
----------
Version 1
  - Initial version

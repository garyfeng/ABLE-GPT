QTE Previewer Integration
-------

The QTE previewer is an experimental QTI v3p0 item and package previewer. The base URL for the dev version is at https://develop--qte.netlify.app

# URL encoded QTI item
QTE has a feature to preview an QTI v3 item with the QTI item encoded as part of the URL. The dev URL is https://develop--qte.netlify.app/?item-base64=`[STRING_HERE]`, 
where `[STRING_HERE]` is Base64 encoded QTIv3 (with xml declaration) then url encoded. In javascript:

```javascript
encodeURIComponent(btoa(qti_xml_content))
```
Equivalent in Python:

```python
# Encode in base64
base64_encoded = base64.b64encode(qti_xml_content.encode()).decode()

# URL encode the base64 string with quote_plus()
url_encoded_base64 = urllib.parse.quote_plus(base64_encoded)

# Generate the full URL
full_url = f"https://develop--qte.netlify.app/?item-base64={url_encoded_base64}"
```
## Example
In the GPT, the following instruction is used to generate this URL:

```
I need to generate an item preview URL for the following QTI test item. The URL needs to be in the format of
https://develop--qte.netlify.app/?item-base64=[STRING_HERE], where the STRING_HERE is the QTI v3p0 xml content
but encoded first in base64 and then encoded with urllib.parse.quote_plus(). Generate and run the python code. 

<?xml version="1.0" encoding="UTF-8"?>
<qti-assessment-item xmlns="http://www.imsglobal.org/xsd/qti/imsqtiasi_v3p0"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                     xsi:schemaLocation="http://www.imsglobal.org/xsd/qti/imsqtiasi_v3p0 
                     https://purl.imsglobal.org/spec/qti/v3p0/schema/xsd/imsqti_asiv3p0_v1p0.xsd"
                     identifier="item1"
                     title="Fraction Representation"
                     time-dependent="false">
    <qti-response-declaration base-type="identifier" cardinality="single" identifier="RESPONSE">
        <qti-correct-response>
            <qti-value>A</qti-value>
        </qti-correct-response>
    </qti-response-declaration>
    <qti-outcome-declaration base-type="float" cardinality="single" identifier="SCORE">
        <qti-default-value>
            <qti-value>1</qti-value>
        </qti-default-value>
    </qti-outcome-declaration>
    <qti-item-body>
        <p>A pizza is cut into 8 equal slices. What fraction represents one slice of the pizza?</p>
        <qti-choice-interaction max-choices="1" response-identifier="RESPONSE">
            <qti-prompt>Select the correct fraction.</qti-prompt>
            <qti-simple-choice identifier="A">1/8</qti-simple-choice>
            <qti-simple-choice identifier="B">1/4</qti-simple-choice>
            <qti-simple-choice identifier="C">1/2</qti-simple-choice>
            <qti-simple-choice identifier="D">3/8</qti-simple-choice>
        </qti-choice-interaction>
    </qti-item-body>
    <qti-response-processing template="http://www.imsglobal.org/question/qti_v3p0/rptemplates/match_correct" />
</qti-assessment-item>
```

The resulting Python code is:
```python
import base64
import urllib.parse

# QTI v3p0 XML content for the test item
qti_xml_content_v3p0 = """
<?xml version="1.0" encoding="UTF-8"?>
<qti-assessment-item xmlns="http://www.imsglobal.org/xsd/qti/imsqtiasi_v3p0"
                     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                     xsi:schemaLocation="http://www.imsglobal.org/xsd/qti/imsqtiasi_v3p0 
                     https://purl.imsglobal.org/spec/qti/v3p0/schema/xsd/imsqti_asiv3p0_v1p0.xsd"
                     identifier="item1"
                     title="Fraction Representation"
                     time-dependent="false">
    <qti-response-declaration base-type="identifier" cardinality="single" identifier="RESPONSE">
        <qti-correct-response>
            <qti-value>A</qti-value>
        </qti-correct-response>
    </qti-response-declaration>
    <qti-outcome-declaration base-type="float" cardinality="single" identifier="SCORE">
        <qti-default-value>
            <qti-value>1</qti-value>
        </qti-default-value>
    </qti-outcome-declaration>
    <qti-item-body>
        <p>A pizza is cut into 8 equal slices. What fraction represents one slice of the pizza?</p>
        <qti-choice-interaction max-choices="1" response-identifier="RESPONSE">
            <qti-prompt>Select the correct fraction.</qti-prompt>
            <qti-simple-choice identifier="A">1/8</qti-simple-choice>
            <qti-simple-choice identifier="B">1/4</qti-simple-choice>
            <qti-simple-choice identifier="C">1/2</qti-simple-choice>
            <qti-simple-choice identifier="D">3/8</qti-simple-choice>
        </qti-choice-interaction>
    </qti-item-body>
    <qti-response-processing template="http://www.imsglobal.org/question/qti_v3p0/rptemplates/match_correct" />
</qti-assessment-item>
"""

# Encode in base64
base64_encoded_v3p0 = base64.b64encode(qti_xml_content_v3p0.encode()).decode()

# URL encode using quote_plus
url_encoded_base64_v3p0 = urllib.parse.quote_plus(base64_encoded_v3p0)

# Generate the full URL
full_url_v3p0 = f"https://develop--qte.netlify.app/?item-base64={url_encoded_base64_v3p0}"

full_url_v3p0
```
The final URL that can launch the QTE item previewer is: [Preview URL](https://develop--qte.netlify.app/?item-base64=Cjw%2FeG1sIHZlcnNpb249IjEuMCIgZW5jb2Rpbmc9IlVURi04Ij8%2BCjxxdGktYXNzZXNzbWVudC1pdGVtIHhtbG5zPSJodHRwOi8vd3d3Lmltc2dsb2JhbC5vcmcveHNkL3F0aS9pbXNxdGlhc2lfdjNwMCIKICAgICAgICAgICAgICAgICAgICAgeG1sbnM6eHNpPSJodHRwOi8vd3d3LnczLm9yZy8yMDAxL1hNTFNjaGVtYS1pbnN0YW5jZSIKICAgICAgICAgICAgICAgICAgICAgeHNpOnNjaGVtYUxvY2F0aW9uPSJodHRwOi8vd3d3Lmltc2dsb2JhbC5vcmcveHNkL3F0aS9pbXNxdGlhc2lfdjNwMCAKICAgICAgICAgICAgICAgICAgICAgaHR0cHM6Ly9wdXJsLmltc2dsb2JhbC5vcmcvc3BlYy9xdGkvdjNwMC9zY2hlbWEveHNkL2ltc3F0aV9hc2l2M3AwX3YxcDAueHNkIgogICAgICAgICAgICAgICAgICAgICBpZGVudGlmaWVyPSJpdGVtMSIKICAgICAgICAgICAgICAgICAgICAgdGl0bGU9IkZyYWN0aW9uIFJlcHJlc2VudGF0aW9uIgogICAgICAgICAgICAgICAgICAgICB0aW1lLWRlcGVuZGVudD0iZmFsc2UiPgogICAgPHF0aS1yZXNwb25zZS1kZWNsYXJhdGlvbiBiYXNlLXR5cGU9ImlkZW50aWZpZXIiIGNhcmRpbmFsaXR5PSJzaW5nbGUiIGlkZW50aWZpZXI9IlJFU1BPTlNFIj4KICAgICAgICA8cXRpLWNvcnJlY3QtcmVzcG9uc2U%2BCiAgICAgICAgICAgIDxxdGktdmFsdWU%2BQTwvcXRpLXZhbHVlPgogICAgICAgIDwvcXRpLWNvcnJlY3QtcmVzcG9uc2U%2BCiAgICA8L3F0aS1yZXNwb25zZS1kZWNsYXJhdGlvbj4KICAgIDxxdGktb3V0Y29tZS1kZWNsYXJhdGlvbiBiYXNlLXR5cGU9ImZsb2F0IiBjYXJkaW5hbGl0eT0ic2luZ2xlIiBpZGVudGlmaWVyPSJTQ09SRSI%2BCiAgICAgICAgPHF0aS1kZWZhdWx0LXZhbHVlPgogICAgICAgICAgICA8cXRpLXZhbHVlPjE8L3F0aS12YWx1ZT4KICAgICAgICA8L3F0aS1kZWZhdWx0LXZhbHVlPgogICAgPC9xdGktb3V0Y29tZS1kZWNsYXJhdGlvbj4KICAgIDxxdGktaXRlbS1ib2R5PgogICAgICAgIDxwPkEgcGl6emEgaXMgY3V0IGludG8gOCBlcXVhbCBzbGljZXMuIFdoYXQgZnJhY3Rpb24gcmVwcmVzZW50cyBvbmUgc2xpY2Ugb2YgdGhlIHBpenphPzwvcD4KICAgICAgICA8cXRpLWNob2ljZS1pbnRlcmFjdGlvbiBtYXgtY2hvaWNlcz0iMSIgcmVzcG9uc2UtaWRlbnRpZmllcj0iUkVTUE9OU0UiPgogICAgICAgICAgICA8cXRpLXByb21wdD5TZWxlY3QgdGhlIGNvcnJlY3QgZnJhY3Rpb24uPC9xdGktcHJvbXB0PgogICAgICAgICAgICA8cXRpLXNpbXBsZS1jaG9pY2UgaWRlbnRpZmllcj0iQSI%2BMS84PC9xdGktc2ltcGxlLWNob2ljZT4KICAgICAgICAgICAgPHF0aS1zaW1wbGUtY2hvaWNlIGlkZW50aWZpZXI9IkIiPjEvNDwvcXRpLXNpbXBsZS1jaG9pY2U%2BCiAgICAgICAgICAgIDxxdGktc2ltcGxlLWNob2ljZSBpZGVudGlmaWVyPSJDIj4xLzI8L3F0aS1zaW1wbGUtY2hvaWNlPgogICAgICAgICAgICA8cXRpLXNpbXBsZS1jaG9pY2UgaWRlbnRpZmllcj0iRCI%2BMy84PC9xdGktc2ltcGxlLWNob2ljZT4KICAgICAgICA8L3F0aS1jaG9pY2UtaW50ZXJhY3Rpb24%2BCiAgICA8L3F0aS1pdGVtLWJvZHk%2BCiAgICA8cXRpLXJlc3BvbnNlLXByb2Nlc3NpbmcgdGVtcGxhdGU9Imh0dHA6Ly93d3cuaW1zZ2xvYmFsLm9yZy9xdWVzdGlvbi9xdGlfdjNwMC9ycHRlbXBsYXRlcy9tYXRjaF9jb3JyZWN0IiAvPgo8L3F0aS1hc3Nlc3NtZW50LWl0ZW0%2BCg%3D%3D)

## Evaluation
In ChatGPT, it does print out the URL in full, but one cannot click on the URL to launch the previewer. Instead, one has to 
copy the URL and paste into a new browser window. In addition, the URL generation is slow (a couple of minutes) because
(1) it needs to generate the code, which includes copying the long QTI xml string, and (2) it needs to copy the final
URL from the code interpreter to the ChatGPT UI, which again is a generative task that takes painfully long time. 

### Quick fix
An URL shortening service would make this process 2x faster. But we'd need an API that we don't want to expose to 
GPT. Sigh.

# Action
An alternative is to create a GPT action, following the OpenAI Function Calling paradigm. This requires a 
RESTful API on the side of the QTE previewer. We then take the QTI generated by the GPT, encode if necessary,
pass it to the API as a payload. The API will return an URL -- shortened ideally -- so that the user can launch. 



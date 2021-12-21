# Snap2Place Model

## Input Data
Supported MIME Content Types:

- `application/json`

### Sample Input
This model takes in a simple JSON payload of latitude and longitude values.

Requirements:

```
Latitude: Float type
Longitude: Float type
Lat/Lng should be within the US region
```

Examples:

For Inference Endpoint, a single JSON Object:
```json
{
   "lat":33.89382700435817,
   "lng":-118.36993614211679
}
```

For Batch Transform, a json file with multiple json objects

```json
{"lat":21.28616324934016,"lng":-157.83882418647408}
{"lat":21.285581002011895,"lng":-157.83880507573485}
{"lat":21.28572198562324,"lng":-157.83898310735822}
```


## Output data
MIME Content Type: `application/json`
For both real-time endpoint and batch transform job, the
outputs are the same.


Sample Output:
The corresponding output are json objects containing an array of VenueIDs and the probability that it is that VenueID. In batch transform, it will output to a file filled with these json objects.

```json
{
    "Venues":[
        ["582378fec4c9fa2d07af3a81", 0.5457066913729076],
        ["4b0dc578f964a520c94f23e3", 0.2762055931455515]
    ]
}
```


## Code example of using model
### AWS CLI Command
After creating an endpoint for real-time inference from the
model package, you can invoke the real-time endpoint using
AWS CLI Endpoint Call:

```bash
aws sagemaker-runtime invoke-endpoint \
--cli-binary-format raw-in-base64-out
--endpoint-name "endpoint-name" \
--body lat-long-json-object \
--content-type "application/json"  \
out.json
```

Substitute the following parameters:

- `endpoint-name` - name of the inference endpoint where
the model is deployed

- `lat-long-json-object` - input image to perform inference on 
    - example: `{"lat":33.89382700435817,"lng":-118.36993614211679}`

- `out.json` - filename where the inference results are written to

### Python
After creating an endpoint for real-time inference from the
model package, you can invoke the real-time endpoint using AWS SDK for Python:

Boto3 Python Call

```python
client = boto3.Session().client(service_name='runtime.sagemaker')
response = client.invoke_endpoint(
            EndpointName='endpoint-name',
            ContentType="application/json",
            Accept="application/json",
            Body='{"lat":33.89382700435817, "lng":-118.36993614211679}'
)
response = response['Body'].read()
```


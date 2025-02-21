Provide your CLI command here:
```bash
jq -r 'select(.symbol == "TSLA") | .order_id' ./transaction-log.txt | xargs -I {} curl -s -X GET https://example.com/api/{} >> ./output.txt
```

# Explanation

## First Operation (Filtering)
```bash
jq -r 'select(.symbol == "TSLA" and .side == "sell") | .order_id' ./transaction-log.txt:
```

* Filters ```./transaction-log.txt``` (JSON format) to find entries where symbol is "TSLA" and side is "sell".
* Extracts the order_id from those entries. 
* ```r``` ensures raw string output (no JSON formatting).
* ```|``` (pipe): Passes the extracted order_id values to the next command.

## Second Operations (Looping)
```bash
xargs -I {} curl -s -X GET https://example.com/api/{}
```
For each order_id, constructs a URL https://example.com/api/{order_id}.
Sends an HTTP GET request to that URL using curl.

* ```s``` suppresses curl output.
* ```-X GET``` explicitly specifies the GET method.
* ```>> ./output.txt```: Appends the response from each curl request to the ./output.txt file.

# Prerequisites
* jq must be installed (```sudo apt-get install jq``` on Debian/Ubuntu)
* transaction-log.txt must exist in the current directory.
* The user must have write permissions to the current directory to create/append to output.txt.

# Notes
* The https://example.com/api/{} URL is a placeholder. Replace it with the actual API endpoint.
* Error handling is minimal (e.g., network errors, invalid JSON).
* The -s option in curl suppresses all output, including errors. Remove it for debugging.
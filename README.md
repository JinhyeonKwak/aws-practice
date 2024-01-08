# aws-practice

## AWS 인스턴스 유형, 패밀리 등에 따른 성능 측정
(t4g 유형의 사이즈에 따른 비교, 인스턴스 패밀리 혹은 프로세서 패밀리에 따른 비교)
### 성능 측정 방법
1. 실행 시간 : 구동되고 있는 서버에 크롤링 api 요청을 한 번에 10개를 보내어 실행 시간을 측정함.
2. CPU 이용률 : 인스턴스 콘솔에서 모니터링 툴을 이용함.
```
#!/bin/bash

# Check if API_URL parameter is provided
if [ -z "$1" ]; then
    echo "Usage: $0 <API_URL>"
    exit 1
fi

# Number of concurrent requests
NUM_CONCURRENT_REQUESTS=10

# Your API endpoint
API_URL="$1"  # Assuming HTTP, change to https if needed

rm -f response_files/response_*.html

# Directory to save response files
RESPONSE_DIR="response_files"

# Create the directory if it doesn't exist
mkdir -p "$RESPONSE_DIR"

# Function to make an HTTP request and save response to a file
make_request() {
    response=$(curl -s "$API_URL")
    if [ -n "$response" ]; then
        filename="$RESPONSE_DIR/response_$RANDOM.html"
        echo "$response" > "$filename"
        echo "$filename"
    else
        echo "Failed to fetch response from $API_URL"
    fi
}

# Simulate concurrent requests and save responses to files
for ((i = 1; i <= NUM_CONCURRENT_REQUESTS; i++)); do
    response_file=$(make_request)
    if [ -n "$response_file" ]; then
        echo "Response for request $i saved to: $response_file"
    fi
done
```
![image](https://github.com/JinhyeonKwak/JinhyeonKwak/assets/93817551/a20db6a1-ccf2-4855-91ce-418d1ddc6437)
![image](https://github.com/JinhyeonKwak/JinhyeonKwak/assets/93817551/4145c3e0-de9f-47ea-90fb-8ccf514bdbfb)

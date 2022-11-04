# Lab Cloud Speech API

gcloud alpha services api-keys create --display-name=API_CLOUD_SPEECH_KEY

export API_KEY="AIzaSyDK94LLDSjPD3PSNwL7fLbejn8XmeQLgnM" # in key_string

touch request.json
nano request.json

## to be put in request.json
```json
{
  "config": {
      "encoding":"FLAC",
      "languageCode": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-samples-tests/speech/brooklyn.flac"
  }
}
```

curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}"

curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > result.json
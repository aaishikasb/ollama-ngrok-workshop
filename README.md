# Workshop- `How to run LLMs locally using ngrok`

## Installation Guide

- **Sign Up for a ngrok Account**
  - https://ngrok.com/signup
- **Install ngrok**
  - https://ngrok.com/download
- **Install Ollama**
  - https://ollama.com/

### Authenticate ngrok

Find your AUTH token on the ngrok dashboard: https://dashboard.ngrok.com/get-started/your-authtoken

```
ngrok config add-authtoken YOUR-NGROK-AUTHTOKEN
```

## Running an LLM using Ollama

- **Choose your LLM from the list here:** https://ollama.com/search
- Run the following in your terminal, replacing `<LLM>` with your choice.
```
ollama run <LLM>
```
  - Example: `ollama run deepseek-r1`
- Run a prompt and check if you're receiving an output. Then kill all processes.

## Deploying on ngrok

- Execute the following in your terminal. This will take up port `11434`.
```
ollama serve
```
- (Optional) Test using the following:
  - ``` bash
    curl --request GET \
    --url http://localhost:11434/
    ```
    Ideal Response: `Ollama is running`
- Run
```
ngrok http 11434 --host-header=localhost
```

> [!NOTE]  
> `--host-header=localhost` is important because Ollama requires this header.

## Testing the ngrok Endpoint

- Copy the ngrok ephemeral URL generated.
- Run the following command:
  - ``` bash
     curl <NGROK-URL>/v1/chat/completions \
    -H "Content-Type: application/json" \
    -d '{
        "model": "deepseek-r1:latest",
        "messages": [
          {
            "role": "system",
            "content": "You are a helpful assistant."
          },
          {
            "role": "user",
            "content": "Hello!"
           }
         ]
       }'
    ```

## Authentication using ngrok

- Create a new Traffic Policy using 
```
nano traffic-policy.yml
```
- Copy and paste the contents from [traffic-policy.yml](traffic-policy.yml).
- Run 
```
ngrok http 11434 --traffic-policy-file traffic-policy.yml --host-header=localhost
```
- To test, execute 
``` bash
   curl --request GET \
     --url <NGROK-URL> \
     --header 'Authorization: Basic dXNlcjpwYXNzd29yZDE='
     ```


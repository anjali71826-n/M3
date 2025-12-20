# INDMoney Fees Agent

## How to use the API
You can test the agent using the following cURL command:

```bash
curl -X POST [https://anjali71826.app.n8n.cloud/webhook/indmoney-fees-agent](https://anjali71826.app.n8n.cloud/webhook/indmoney-fees-agent) \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer abcde" \
  -d '{
    "user_query": "What are the charges for US stocks?",
    "session_id": "test_session_1"
  }'
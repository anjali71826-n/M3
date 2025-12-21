INDmoney Fees Agent - API Workflow DocumentationThis document outlines the sequential execution of the INDmoney Fees Agent webhooks. The workflow consists of three distinct steps: initiating the query, providing specific context (using a Session ID), and triggering a final approval.Workflow OverviewsequenceDiagram
    participant Client
    participant Agent_Webhook as /indmoney-fees-agent
    participant Continue_Webhook as /indmoney-fees-continue
    participant Approve_Webhook as /indmoney-fees-approve

    Note over Client, Agent_Webhook: Step 1: Initial Query
    Client->>Agent_Webhook: POST { user_query }
    Agent_Webhook-->>Client: Returns JSON { session_id, ... }
    
    Note over Client, Continue_Webhook: Step 2: Context & Session Handling
    Client->>Continue_Webhook: POST { session_id, answers, original_query }
    Continue_Webhook-->>Client: Returns Acknowledgement/Result

    Note over Client, Approve_Webhook: Step 3: Final Approval
    Client->>Approve_Webhook: GET Request
    Approve_Webhook-->>Client: Approval Confirmation
Step 1: Initiate QueryEndpoint: POST /webhook/indmoney-fees-agentThis step starts the conversation. The response from this API call is critical as it generates the session_id required for the next step.Requestcurl -X POST [https://anjali71826.app.n8n.cloud/webhook/indmoney-fees-agent](https://anjali71826.app.n8n.cloud/webhook/indmoney-fees-agent) \
  -H "Content-Type: application/json" \
  -d '{
    "user_query": "What are the charges for US stock trading?"
  }'
Response HandlingAction: Capture the session_id from the JSON response body.Variable: current_session_idStep 2: Provide Specific ContextEndpoint: POST /webhook/indmoney-fees-continueThis step passes specific user intent ("SIP") linked to the original query using the session ID obtained in Step 1.Request# Note: Replace SESSION_ID_FROM_STEP_1 with the actual value received
curl -X POST [https://anjali71826.app.n8n.cloud/webhook/indmoney-fees-continue](https://anjali71826.app.n8n.cloud/webhook/indmoney-fees-continue) \
  -H "Content-Type: application/json" \
  -d '{
    "session_id": "SESSION_ID_FROM_STEP_1",
    "answers": "I want to invest in US stocks via SIP",
    "original_query": "What are the charges for US stock trading?"
  }'
Step 3: ApprovalEndpoint: GET /webhook-test/indmoney-fees-approveThis final step triggers the approval process. It is a simple GET request.Requestcurl -X GET [http://anjali71826.app.n8n.cloud/webhook-test/indmoney-fees-approve](http://anjali71826.app.n8n.cloud/webhook-test/indmoney-fees-approve)
Implementation Example (JavaScript/Node.js)Below is an example of how to execute these requests sequentially using JavaScript, ensuring the session_id is passed correctly.const executeWorkflow = async () => {
  try {
    // --- Step 1: Initial Query ---
    console.log("Step 1: Initiating Query...");
    const step1Response = await fetch('[https://anjali71826.app.n8n.cloud/webhook/indmoney-fees-agent](https://anjali71826.app.n8n.cloud/webhook/indmoney-fees-agent)', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        "user_query": "What are the charges for US stock trading?"
      })
    });

    const step1Data = await step1Response.json();
    
    // Extract Session ID (Assumes response structure contains session_id at root)
    // Adjust path based on actual n8n response structure, e.g., step1Data.body.session_id
    const sessionId = step1Data.session_id || "session_1765818756946"; 
    
    console.log(`Received Session ID: ${sessionId}`);

    // --- Step 2: Continue with Context ---
    console.log("Step 2: Sending Follow-up...");
    await fetch('[https://anjali71826.app.n8n.cloud/webhook/indmoney-fees-continue](https://anjali71826.app.n8n.cloud/webhook/indmoney-fees-continue)', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        "session_id": sessionId,
        "answers": "I want to invest in US stocks via SIP",
        "original_query": "What are the charges for US stock trading?"
      })
    });

    // --- Step 3: Approval ---
    console.log("Step 3: Triggering Approval...");
    const approvalResponse = await fetch('[http://anjali71826.app.n8n.cloud/webhook-test/indmoney-fees-approve](http://anjali71826.app.n8n.cloud/webhook-test/indmoney-fees-approve)', {
      method: 'GET'
    });

    console.log("Workflow Complete. Approval Status:", approvalResponse.status);

  } catch (error) {
    console.error("Workflow failed:", error);
  }
};

executeWorkflow();
